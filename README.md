# A Fast Tsetlin Machine Implementation Employing Bit-Wise Operators
An implementation of the Tsetlin Machine (https://arxiv.org/abs/1804.01508) using bitwise operations for increased learning- and classification speed.

On the MNIST dataset, the bit manipulation leads to approx.
* 10 times smaller memory footprint,
* 8 times quicker classification, and
* 10 times faster learning,

compared to the vanilla Cython (https://github.com/cair/TsetlinMachine) and C (https://github.com/cair/TsetlinMachineC) implementations.

## Bit-Based Representation and Manipulation of Patterns

The Tsetlin Machine solves complex pattern recognition problems with propositional formulas, composed by a collective of Tsetlin Automata. In this implementation, we express both inputs, patterns, and outputs as bits, while recognition and learning rely on bit manipulation. Briefly stated, the states of the Tsetlin Automata are jointly represented using multiple sequences of bits (e.g., 8 sequences to represent an 8 bit state index). Sequence 1 contains the first bit of each state index. Sequence 2 contains the second bit, and so on, as exemplified below for 24 Tsetlin Automata:

![Figure 4](https://github.com/olegranmo/blob/blob/master/Bit_Manipulation_3.png)

The benefit of this representation is that the action of each Tsetlin Automaton is readily available from the most significant bit (sequence 8 in the figure). Thus, the output (recognized or not recognized pattern) can be obtained from the input based on fast bitwise operators (NOT, AND, and CMP - comparison). When deployed after training, only the sequence containing the most significant bit is required. The other sequences can be discarded because these bits are only used to keep track of the learning. This provides a further reduction in memory usage.

## MNIST Demo
```bash
unzip BinarizedMNISTData.zip
make
./MNISTDemoBits 

EPOCH 1
Training Time: 33.1 s
Evaluation Time: 13.3 s
Test Accuracy: 94.42
Training Accuracy: 95.99

EPOCH 2
Training Time: 25.8 s
Evaluation Time: 13.8 s
Test Accuracy: 95.39
Training Accuracy: 96.72

EPOCH 3
Training Time: 24.6 s
Evaluation Time: 13.9 s
Test Accuracy: 95.77
Training Accuracy: 97.21

EPOCH 4
Training Time: 23.9 s
Evaluation Time: 13.9 s
Test Accuracy: 96.27
Training Accuracy: 97.54

EPOCH 5
Training Time: 23.3 s
Evaluation Time: 14.2 s
Test Accuracy: 96.52
Training Accuracy: 97.76
...

EPOCH 396
Training Time: 18.5 s
Evaluation Time: 15.0 s
Test Accuracy: 98.15
Training Accuracy: 99.90

EPOCH 397
Training Time: 18.3 s
Evaluation Time: 14.5 s
Test Accuracy: 98.12
Training Accuracy: 99.91

EPOCH 398
Training Time: 18.2 s
Evaluation Time: 14.4 s
Test Accuracy: 98.13
Training Accuracy: 99.92

EPOCH 399
Training Time: 18.4 s
Evaluation Time: 14.9 s
Test Accuracy: 98.19
Training Accuracy: 99.90

EPOCH 400
Training Time: 18.0 s
Evaluation Time: 14.4 s
Test Accuracy: 98.12
Training Accuracy: 99.90
```
## Data Preparation

The included dataset is a binarized, but otherwise unenhanced, version of the MNIST dataset (http://yann.lecun.com/exdb/mnist/), downloaded from https://github.com/mnielsen/neural-networks-and-deep-learning/tree/master/data. The complete dataset was binarized by replacing pixel values larger than 0.3 with 1 (with the original pixel values ranging from 0 to 1). Pixel values below or equal to 0.3 were replaced with 0. The following image is an example of the images produced.

```bash
............................
............................
............................
............................
............................
............................
............@@..............
...........@@.....@@@.......
...........@@.....@@........
..........@@......@@........
.........@@@......@@........
.........@@.......@@........
.........@@......@@@........
........@@@.@@@..@@@........
........@@@@@@@@@@@@........
.......@@@@@@@@@@@@.........
.......@@........@@.........
.................@@.........
................@@@.........
................@@@.........
................@@@.........
................@@@.........
................@@@.........
................@@@.........
................@@@.........
................@@@.........
............................
............................
```
## Learning Behaviour
The below figure depicts average learning progress (across 50 runs) of the Tsetlin Machine on the included MNIST dataset.

![Figure 4](https://github.com/olegranmo/blob/blob/master/learning_progress.png)

As seen in the figure, both test and training accuracy increase almost monotonically across the epochs. Even while accuracy on the training data approaches 99.9%, accuracy on the test data continues to increase as well, hitting 98.2% after 400 epochs. This is quite different from what occurs with backpropagation on a neural network, where accuracy on test data starts to drop at some point due to overfitting, without proper regularization mechanisms.

## Further Work

* Perform a more extensive hyperparameter search (manipulating THRESHOLD, CLAUSES, STATE_BITS, and S in TsetlinMachineBitsConfig.h).
* Evaluate different binarization and data augmentation approaches for MNIST, including deskewing, noise removal, blurring, and pixel shifts.
* Investigate effect of using an ensemble of Tsetlin Machines.
* Optimize code base further.

## Citation

```bash
@article{granmo2018tsetlin,
  author = {{Granmo}, Ole-Christoffer},
  title = "{The Tsetlin Machine - A Game Theoretic Bandit Driven Approach to Optimal Pattern Recognition with Propositional Logic}",
  journal={arXiv preprint arXiv:1804.01508},
  year={2018}
}
```

## Licence

Copyright (c) 2019 Ole-Christoffer Granmo

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.

