## Medical Image Reconstruction Project

In 2021 I was presented with a challenge: develop a project, all by myself, on a topic I liked! I immediately thought about using the knowledge I had gained in my bachelor's degree and apply it to one of my passions, biomedics. 

I ended up choosing medical Image Reconstruction as my project. I chose it since developing new mathematical tools to treat and vizualise data is really important nowadays to improve disease diagnosis and study the human body and brain. 

Knowing nothing about this topic I soon started investigating on how medical images were being produced. I discovered pretty quickly that the mathematical basis was the discrete Fourier Transform. Of course a straightforward implementation could be readily made: imagine we start with a complex sequence of lenght _n_; to perform its transform we simply compute _n_ sums, each of length _n_. But this was too slow of a computation: _n_ squared operations.

### Cooley-Tukey FFT

The next step was to investigate how to optimize this computation. I found out we can make use of fast Fourier Transform algorithms. The most common FFT is the _Cooley-Tukey_ algorithm. It consists on a divide and conquer algorithm such that the lenght-_n_ transform is replaced by two transforms of lenght _n_/2. 

### Two different Implementations

I implemented two methods: a recursive and an iterative algorithm. The recursive algorithm needed _O(n)_ function calls to itself. These calls can be replaced by the iterative algorithm.

The idea for this last method lies on swapping elements whose indices are binary reversals. First we swap the mutual reversals: for example, the element in position 1 (001 in binary) winds up in position 4 (100 in binary). Elements whose index is the  same reversed stay put, like index 2. Then we merge adjacent sublists where every sublist represents the half-size transform of the even or odd part of the full size transform.

### 2D FFT

All these methods were tested in 1-dimensional data sets using MATLAB. Next I focused on a 2-dimensional FFT. The 2-dimensional transform can be cmputed by applying 1D transforms. Let's assume we have a 2D array. We can treat this array as an _N1xN2_ matrix. Then it is as easy as applying a 1D transform first on the rows and then on the columns - _row-column algorithm_.

### Graphic Interface

I then built a graphic interface that can be used to import raw k-space data files, perform the FFT for 1 and 2 dimensional data and visualize the results. You can see below the general aspect of this interface:

![Captura de Ecrã (13)](https://user-images.githubusercontent.com/80169619/134490927-daacf9ab-db0e-42d9-b2da-40b82195b3bb.png)

There are three main panels: one containing all the tools necessary to operate the program (on the left), another showing all the Input information (middle section) and an Output panel that will show the resulting images. 

It is really easy to use:

1. File -> Open data file
2. Modify your Input data as necessary
3. Select the FFT algorithm you wish to use
4. Calculate!

### Input Modifications

There are important things to be considered regarding the input data. 

First notice that the left panel will show a description of the file opened and data dimensions. For both recursive and iterative implementations the data size should be a power of two. If that is not the case you should use the 'Pad with 0s' option. 

![novopaddd](https://user-images.githubusercontent.com/80169619/134494527-37726734-3630-45a2-919f-dc3cdfa400c9.PNG)

Also, if your input file does not have the zero frequency centered, the output will appear in the corners and we don't want that. For that purpose there is a 'Shift to Center' option.

![shiftcenter](https://user-images.githubusercontent.com/80169619/134494969-2cdb7b57-751d-4c52-9010-e7c5ef809853.PNG)

### Other options

The 'Display Options' allows the user to resize the input image (in _x_ and _y_) and control the contrast with the 'Logaritmic gradient' and 'Per layer gradient' checkboxes. The later makes the color space only consider the current layer values instead of the whole domain.

Finallly, one can choose to show or hide thae tables with numerical data - 'Show Data Tables' checkbox.

All these options are also available in the Output section.

### Results

I tested with several frequency data obtained from open sources. Here are the reconstruction results!

![peemao](https://user-images.githubusercontent.com/80169619/134503879-d1cd5a01-752e-4530-a59c-8abeda95c405.PNG)
<p align="center">
  <img width="460" height="300" src="https://user-images.githubusercontent.com/80169619/134503900-7b8c43d8-ba96-45f1-8024-b15383e2974f.PNG">
</p>

### Computation Times

Although this project was not focused on developing the most optimized algorithms, comparing the computation times allows us to verify the speed gains of the recursive and iterative approaches.

![tabela](https://user-images.githubusercontent.com/80169619/134504852-5dd00c51-a56f-4a2d-8a8f-214710ffb2f7.PNG)