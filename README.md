# CNN-for--seismic-fault-detection-using-Pytorch-based-UNet

Seismic surveys are fundamental in the exploration of oil and natural gas resources, both on land and in offshore areas. These surveys have significantly reduced exploration costs and enabled the discovery of reserves that were previously undetectable through traditional methods. The seismic survey process can be divided into two primary stages: data collection and seismic interpretation. During data collection, energy sources and an array of sensors are used to record seismic waves as they travel through the earth. This recorded data is then processed and prepared for seismic interpretation.

Fault detection plays a crucial role in this process, as it directly impacts the success of locating hydrocarbon reserves beneath the surface. Traditionally, faults are identified as discontinuities in reflections or abrupt changes in post-stack seismic data, a process that is both labor-intensive and time-consuming.

To enhance the efficiency of this process, various automatic fault detection techniques have been proposed, with deep learning methods gaining the most attention. These methods typically require very large, labeled datasets for training, which may not be feasible for smaller companies that struggle to acquire enough data. To address this challenge, we implement a transfer learning approach that enables the use of smaller datasets for training fault detection models. We begin by training a deep neural network on synthetic seismic data.

Next, the network is fine-tuned using real seismic data. To achieve this, we use the Random Sample Consensus (RANSAC) algorithm to extract real seismic samples and generate corresponding labels automatically. We evaluate the method with three real 3D examples, demonstrating that retraining the model with a limited set of real seismic samples significantly enhances the fault detection performance of the pre-trained models.


#Introduction


A seismic survey is carried out over the area of interest using reflective seismology, a technique in which an energy source generates a signal or wave that penetrates the earth's subsurface. As the waves travel through the rock layers, they behave differently depending on the material properties, either reflecting, refracting, or diffracting at varying speeds. These waves are detected by geophones positioned on the surface, and the travel times are recorded. After this data is processed, the seismic interpreter's role is to identify and map the geological faults present in the area.

![image](https://github.com/user-attachments/assets/81800405-e1a0-4c30-ab6b-4d508dfa4089)


#Seismic Waves



Faults are geological features formed by a combination of various processes, including tectonic plate movements, gravity, and overpressures. These fractures or planes allow rock blocks to shift along them and can vary in size from meters to kilometers. Faults are significant in oil and gas exploration, as they can serve as natural traps for hydrocarbons and also facilitate their migration. In addition to aiding in hydrocarbon discovery, fault mapping is a crucial part of reservoir characterization, which seeks to enhance understanding of the reservoir's internal structure and assist in calculating vital economic metrics.


![Basic-of-seismic-oil-exploration](https://github.com/user-attachments/assets/b918572e-7eae-48e1-8335-1d1d7fd23842)


#Seismic Faults


Traditional fault detection methods require seismic interpreters to manually trace and label faults, a time-consuming process that can take anywhere from weeks to months to complete for a typical area of interest. Due to its inefficiency, many researchers have explored alternative approaches to improve the interpretation process. As faults create discontinuities and abrupt changes, some techniques use statistical models to identify them. However, relying on only a few physical properties proves to be limited and often ineffective in accurately detecting faults. Consequently, the most effective detection techniques employ machine learning and deep neural networks (DNNs), which provide fast and reliable results for fault identification.


![192174382-d00e7ef3-be31-4558-a813-49f584b1782b](https://github.com/user-attachments/assets/3322172a-f43e-4f4f-ab27-29c872fa183a)


#An example of a seismic image


The convolutional neural network (CNN) is a widely used deep neural network (DNN) that has shown great effectiveness in various tasks. However, training a CNN properly requires a large number of processed and labeled samples, often numbering in the thousands or even millions. Since acquiring seismic data is highly costly, smaller companies in the industry typically lack access to such extensive datasets. Addressing this limitation is critical, as it directly influences the cost of exploration. In this study, we propose a strategy that leverages synthetic data alongside a small set of real data to train a CNN built on the U-Net architecture for automatic fault detection. In practical scenarios, this approach would mean that a seismic interpreter only needs to provide a few labeled samples for training the network, after which the model can automatically identify and label the remaining faults in the dataset.


#Literature Review


A seismic image offers a snapshot of the Earth's subsurface structure at a specific moment in time. A "source" generates a sound wave that travels into the Earth’s layers at varying speeds, and these waves can be reflected, refracted, or diffracted as they move through different materials. In seismic imaging, the waves reflected from various geological layers are recorded and stacked to create a 2D or 3D representation. Since each geological layer has unique physical properties, the waves are reflected at the boundary between layers due to contrasts in density. While there are several types of seismic waves, P-waves (compressional waves) are primarily used in imaging. The image below illustrates an example of seismic data acquisition and the resulting final image after all the reflected waves are collected.


![192174533-dab31c2b-86e7-403c-8fda-86643debae1b](https://github.com/user-attachments/assets/1ad79416-b666-4e5a-8d15-26ba77c386e4)

#Data Exploration


This study's data is in the SEG-Y format, which is the industry standard for seismic data (SEG stands for Society of Exploration Geophysicists). This is a specialized data format, and we used a Python library called 'segyio' that was designed to read it. Because we are working with a 3D dataset, the segyio can easily convert it to a 3D NumPy array. A 3D grid structure example is shown below, along with three planar surfaces: inline, crossline, and z-slice. These are the 2D surfaces that are commonly used in the seismic industry to visualize data.


![image](https://github.com/user-attachments/assets/278d5e2e-af4b-4060-9c02-e7608a72278e)

#Training Data

For data set: https://nextcloud.drgaff.net/index.php/s/7mXn8mSbjxdZyr8#


For this study, we will use two volumes of data: a seismic cube and a fault cube, with the seismic cube serving as training data and the fault cube serving as label data. The fault data consists of a series of manually mapped surfaces with values between 0 and 1. The image on the left shows a 2D seismic display in the inline direction, while the image on the right shows the same display with faults overlaid.
#Result
The images below were gathered at random at three different epochs. In Epoch 4, the model is already beginning to detect faults, and by Epoch 19, it has successfully mapped the fault.


![192174915-0ec035d8-4d7d-431e-8504-0407295a1727](https://github.com/user-attachments/assets/1d4cc936-b486-4c69-b224-d8ea23235bf5)


When we look at the model performance, we can see that the loss function drops dramatically within the first 5 epochs and then stabilizes around the 15th epoch. This rapid decrease in model loss is most likely due to the use of clean synthetic data for training.

![192175419-b0165a90-c562-46b0-88c2-dc65dd6901bf](https://github.com/user-attachments/assets/f6ded2d5-281e-4270-afc5-6f5bf7567a34)

Plot between UNet Loss vs Epoch

#Conclusion and Recommendations


This small experiment demonstrated the power of deep learning and how it can be used to map faults relatively quickly when the input data is noise-free. However, in the real world, seismic data are very messy and full of noises, which can significantly impair model accuracy. If we train the model with a wide variety of seismic datasets from various basins all over the world, the model can generalize quite well.
