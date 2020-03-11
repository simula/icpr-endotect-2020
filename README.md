# ICPR EndoTect 2020

## What is this challenge about?
The human digestive system is prone to suffer from many different diseases and abnormalities throughout a human lifetime. Some of these may be life-threatening and pose a serious risk to a patient's health and well-being. In most cases, if the detection of lethal disease is done early enough, it can be treated with a high chance of being fully healed. Therefore, it is important that all lesions are identified and reported during a routine investigation of the GI tract. Currently, the gold-standard in performing these investigations is through video endoscopies, which is a procedure involving a small camera attached to a tube that is inserted either orally or rectally. However, there is one major downside to this procedure. The method is highly dependent on the skill and experience of the person operating the endoscope, which in turn results in a high operator variation and performance. This is one of the reasons for high miss-rates when measuring polyp detection performance, with some miss-rates being as high as 20%. We see this as an opportunity to aid medical doctors by helping them detect lesions through automatic frame analysis done live during endoscopy examinations. The pattern recognition community has a lot of knowledge which could assist in this task, making it a perfect fit for ICPR. The work done in this competition has the potential of making a real societal impact, as it directly affects the quality of care that health-care professionals can provide. 

## What data is provided?
The dataset can be split into four distinct parts; Labeled image data, unlabeled image data, segmented image data, and annotated video data. Each part is further described below. In total, the dataset contains 110,079 images and 373 videos where it captures anatomical landmarks and pathological and normal findings. The results is more than 1,1 million images and video frames all together.

**Labeled images** In total, the dataset contains 10,662 labeled images stored using the JPEG format. The images can be found in the images folder. The classes, which each of the images belongto, correspond to the folder they are stored in (e.g., the "polyp" folder contains all polyp images, the "barretts" folder contains all images of Barrett’s esophagus, etc.). The number of images per class are not balanced, which is a general challenge in the medical field due to the fact that some findings occur more often than others. This adds an additional challenge for researchers, since methods applied to the data should also be able to learn from a small amount of training data. The labeled images represent 23 different classes of findings.

**Unlabeled Images** In total, the dataset contains 99,417 unlabeled images. The unlabeled images can be found in the unlabeled folder which is a subfolder in the image folder, together with the other labeled image folders. In addition to the unlabeled image files, we also provide the extracted global features and cluster assignments in the Hyper-Kvasir Github repository as Attribute-Relation File Format (ARFF) files. ARFF files can be opened and processed using, for example, the WEKA machine learning library, or they can easily be converted into comma-separated values (CSV) files.

**Segmented Images** We provide the original image, a segmentation mask and a bounding box for 1,000 images from the polyp class. In the mask, the pixels depicting polyp tissue, the region of interest, are represented by the foreground (white mask), while the background (in black) does not contain polyp pixels. The bounding box is defined as the outermost pixels of the found polyp. For this segmentation set, we have two folders, one for images and one for masks, each containing 1,000 JPEG-compressed images. The bounding boxes for the corresponding images are stored in a JavaScript Object Notation (JSON) file. The image and its corresponding mask have the same filename. The images and files are stored in the segmented images folder. It is important to point out that the segmented images have duplicates in the images folder of polyps since the images were taken from there.

**Annotated Videos** The dataset contains a total of 373 videos containing different findings and landmarks. This corresponds to approximately 11.62 hours of videos and 1,059,519 video frames that can be converted to images if needed. Each video has been manually assessed by a medical professional working in the field of gastroenterology and resulted in a total of 171 annotated findings.

## Image Labels
Hyper-Kvasir includes the follow image labels for the labeled part of the dataset:

| ID | Label | ID | Label
| --- | --- | --- | --- |
| 0  | barretts | 12 |  oesophagitis-b-d
| 1  | bbps-0-1 | 13 |  polyp
| 2  | bbps-2-3 | 14 |  retroflex-rectum
| 3  | dyed-lifted-polyps | 15 |  retroflex-stomach
| 4  | dyed-resection-margins | 16 |  short-segment-barretts
| 5  | hemorrhoids | 17 |  ulcerative-colitis-0-1
| 6  | ileum | 18 |  ulcerative-colitis-1-2
| 7  | impacted-stool | 19 |  ulcerative-colitis-2-3
| 8  | normal-cecum | 20 |  ulcerative-colitis-grade-1
| 9  | normal-pylorus | 21 |  ulcerative-colitis-grade-2
| 10 | normal-z-line | 22 |  ulcerative-colitis-grade-3
| 11 | oesophagitis-a |  |  |

## What are the tasks?
We propose that the challenge consists of three different tasks, each targeting a different requirement for in-clinic use.

### Detection Task
The detection task stems from the requirement of the high detection accuracy needed to be viable for use in a clinical setting. Participants are asked to develop algorithms that achieve high classification scores on the 23 different classes present in the labeled part of the dataset. Each submission should consist of a single ".csv" file, which contains one line per image in the unlabeled dataset. Each line should start with the name of the predicted file, followed by the predicted label, and then end with the model's confidence of that prediction.

### Efficient Detection Task
The efficient detection task focuses on the real-time analysis needed to deliver instant feedback to doctors performing endoscopies. To satisfy this requirement, the algorithm must achieve good classification scores while also being able to classify images as fast as they are put on screen, which is approximately 30 frames per second. Submissions to this task require the participants to submit their code in the form of a Docker image so that we, the organizers, can evaluate the submissions on the same hardware. The docker image should produce a ".csv" file similar to that of the detection task, with the addition of the processing time being added to the end of each line.

### Segmentation Task
In the segmentation task, we ask participants to use segmented images provided in the dataset to generate segmentations of polyps automatically. Polyps are clumps of cells that form on the mucosal wall of the GI tract. They are among the most important findings in an endoscopy procedure as they are a precursor to different cancer types, including colorectal cancer, which is one of the most lethal cancer types. Submissions to this task will be a ".csv" file consisting of a mapping of the original filename to a generated binary segmentation mask. The ".csv" file should contain one line for each image contained in the unlabeled dataset.

## How is the performance measured?
The goal of each task presented above is different, so we will use different metrics to evaluate each. Please note that for each task, we will use a manually-selected subset of the unlabeled data to evaluate each task.

### Detection Task
For the detection task, we use several standard metrics commonly used to evaluate classification tasks such as precision, recall/sensitivity, specificity, F1, and MCC for multi-classification (also called Rk statistic). The officially reported metric for evaluating this task is the MCC, which therefore also will be the metric used to rank the submissions.

Each metric is further explained below:

* **True positive (TP)**<br>The number of correctly identified samples. The number of frames with an endoscopic finding which correctly is identified as a frame with an endoscopic finding.
* **True negative (TN)**<br>The number of correctly identified negative samples, i.e., frames without an endoscopic finding which correctly is identified as a frame without an endoscopic finding.
* **False positive (FP)**<br>The number of wrongly identified samples, i.e., a commonly called a "false alarm". Frames without an endoscopic finding which is erroneously identified as a frame with an endoscopic finding.
* **False negative (FN)**<br>The number of wrongly identified negative samples. Frames without an endoscopic finding which erroneously is identified as a frame with an endoscopic finding.
* **Recall (REC)**<br>This metric is also frequently called sensitivity, probability of detection and true positive rate, and it is the ratio of samples that are correctly identified as positive among all existing positive samples.
* **Precision (PREC)**<br>This metric is also frequently called the positive predictive value, and shows the ratio of samples that are correctly identified as positive among the returned samples (the fraction of retrieved samples that are relevant).
* **Specificity (SPEC)**<br>This metric is frequently called the true negative rate, and shows the ratio of negatives that are correctly identified as such (e.g., the fraction of frames without an endoscopic finding are correctly identified as a negative result).
* **Accuracy (ACC)**<br>The percentage of correctly identified true and false samples.
* **Matthews correlation coefficient (MCC)**<br>MCC takes into account true and false positives and negatives, and is a balanced measure even if the classes are of very different sizes.
* **F1 score (F1)**<br>A measure of a test’s accuracy by calculating the harmonic mean of the precision and recall.

### Efficient Detection Task
ask For the efficient detection task, we ask participants to submit a docker image so that we can evaluate the speed and efficiency of the proposed algorithm. Here, we will use a combination of the classification score and the number of frames processed per second to evaluate each submission. The focus here is on the "speed" aspect of the algorithm, so the only requirement from a classification standpoint is that it exceeds a set MCC threshold so that it is still viable for in-clinic use.

### Segmentation Task
For the segmentation task, we will use the standard metrics commonly used to evaluate segmentation tasks. This includes the Dice coefficient, pixel accuracy, and the Intersection-Over-Union (Jaccard index). The metric which will be used to rank submissions will be the Dice coefficient.

## Who are the task organizers?
* Steven Hicks, steven@simula,no, SimulaMet & OsloMet
* Debesh Jha, debesh@simula.no, SimulaMet & University of Tromsø
* Hugo Hammer, hugoh@oslomet.no, SimulaMet & OsloMet
* Pål Halvorsen, paalh@simula.no, SimulaMet & OsloMet
* Michael Riegler, michael@simula.no, SimulaMet

## Where to get more information?
If you need help or have any questions please contact Steven Hicks, Pål Halvorsen, or Michael Riegler. Please mark the email with *ICPR EndoTect 2020*.