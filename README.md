# PCB-Visual-Inspection-on-Raspberry-Pi
[C#][UWP] PCB Visual Inspection Device based on Raspberry Pi 3B (Windows IoT)


# Idea:
My idea was born during my internship at LACROIX Electronics. It's a electronics manufacturer company which produces parts for Bosch, VALEO, etc. One of the biggest devices there was SMT Wave Soldering Machine which solders components like resistors, capacitors, chips or LEDs. At each step of soldering there were a minicomputers with normal and IR cameras which have been doing visual inspection of PCBs. The algorithm was checking if the components are not missing or to much shifted. Considering the experience I've gained there and my knowledge and intrest in programming, I proposed my BSc thesis to design, build and program simplier version of that device (without soldering) which performs a visual inspection of PCBs.

# Topics covered:
Image processing, image recognition, numerical methods, IoT (Internet of Things), Raspberry Pi, .NET API, object-oriented programming

# Project:
There were two different approches to make this application on Raspberry Pi. The first one was to make app for Linux and written in Python, The second one was to make app for Windows 10 IoT and written in one of supported programming languages in UWP (.NET Core). Before I proposed my BSc thesis I didn't know anything about .NET but I had been working on C projects so I have chosen C# because its structure and writing style is similiar to C but with more advanced features like object-oriented programming or asynchronus programming and I took the challange made by myself to learn C#. Windows 10 IoT doesn't support other API than UWP so here the choice was limited. Also IoT solutions are widely used in manufacturer companies like LACROIX and there is a demand for IoT solutions in places like that.

# Functions:
At current stage this application works under certain conditions - the camera needs to be 10 cm above white surface and for better exposure additional cold white lightning is required (in engineer's project there are 2 USB LEDs to provide better lightning conditions). Good advantage of using UWP API is that my application can be run not only on Windows 10 IoT but also on desktop Windows 10, Windows 10 Mobile, Xbox One or even on mixed-reality headsets. The main purpose of my program is to validate if on the test image and reference image are the same PCBs. The app has prebuilded two boards for visual inspection (PCB1 - original Adruino UNO Rev3 and PCB2 - chineese UNO Board). The third slot for a reference image is empty and the user can save custom board image by taking picture in a program (there is also camera preview and button for saving PCB3 board) or by putting BMP file in "Pictures(\Camera Roll)" folder in desktop or IoT version of Windows 10. Three different image checking algoritms were prepared for this project. The "Raw" algorithm is comparing pixels of the same coordinates with color tolerance that can be set by the user for three different color channels (red, green and blue). The "Simplified" algorithm as the name suggests simplifies colors of the picture (beside background) from 256^3 colors to 27, 64 or 125 colors and then compares pictures but without color tolerance. The "Edge" algorithm calculates Discreet Convolution of two matrixes (the first matrix is a 2D array of image and the second is a 2D array of filter with one of three operator method). The user can choose what edge detection method will be used. Available methods are: with Sobel operator in 2 directions; with Prewitt operator in 2 directions; with Kirsch operator in 2 directions. Additionally there is a checkbox with "Only strong edges" which outputs the image only with large gradient. There are also extra settings where test threshold can be changed, background filtration can be turned on/off and image can be scalled down. The result of the test can be shown (good/bad/background pixel count and percentage score) as well as processed images of the reference and test boards. For each board the user can zoom in pictures of unprocessed image, simplified image by 2nd algorithm and PCB's edges image by 3rd algorithm. There is also a heat map pictures which presents difference between images (green color - low difference, more red color - greater difference) and a score map which presents which pixels are considered as good (is within tolerance) pixel (green color), bad pixel (red color) or background pixel (blue color).

# Structure:
The project contains of 3 pages and 3 classes. On MainPage there are checkboxes to choose which algorithm will be executed, camera preview and buttons (Start test, Save PCB3, Exit). From this page the user can navigate to Settings Page (SecondPage) or to Report Page (ThirdPage). On SecondPage the user can choose which PCB will be tested and there are many comboboxes&checkboxes with settings for each algorithm and with parameters of testing method. There is also a hyperlink button that will navigate to Camera Preview (MainPage). On ThirdPage there are original and processed images of reference and tested board that can be zoomed in and also there are detailed information good/bad pixel count and overall score. From this page the user can navigate to Camera Preview (MainPage). As the name suggets CameraControl class is used to initialize camera preview and take pictures. In the ImageProcessing class there are methods that transform image to arrays, transform arrays to image, compare pictures, check background, create heatmap & scoremap and save image to bitmap file. In the EdgeDetection class there are matrixes for 3 operators (Sobel, Perwitt and Kirsch) and method that performs convolution filtering to obtain edges from the picture. 

# Summary:
The goal to create application that performs visual inspection of PCBs has been achieved. Accuracy of the algoritms range around 80-90% and It could be better if more expensive camera would be installed. The biggest disadvantages are: webcam and tested board must be in fixed position in order to get the best results; different lightning condintions can affect correctness of the algorithms; the algorithms perform much slower on Raspberry Pi with Windows 10 IoT than on desktop PC. The first two drawbacks come from construction of SMT Soldering Machines. They have conveyor so the position of the board are always almost the same and due to its closed housing the lightning conditions can be controlled easier. Given the fact that Windows 10 IoT is still an experimental version, the performance of the program can be improved with future updates.

# Possible improvments:
- Creating log files;
- Using MessageBox class for some errors;
- Implementation of text algorithm (4th algorithm);
- Better webcam which focus and contrast can be set (and saving this settings to array for tested and reference PCB so they can be compared with the same focus and contrast settings);
- Algorithm that consider and correct light color (used when lighting condition is different);
- Multithreaded programming (dividing pictures to each thread);
