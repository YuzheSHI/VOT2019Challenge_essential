# VOT2019_Essential（要点总结）

## Challenges

(i) `VOT- ST2019` challenge focused on short-term tracking in `RGB`, 
(ii) `VOT-RT2019` challenge focused on “real-time” short- term tracking in `RGB`, 
(iii) `VOT-LT2019` focused on long- term tracking namely coping with target disappearance and reappearance. Two new challenges have been introduced: 
(iv) `VOT-RGBT2019` challenge focused on short-term tracking in `RGB` and thermal imagery
(v) `VOT-RGBD2019` challenge focused on long-term tracking in `RGB` and depth imagery. 

２０１９五大赛道：三通道短视频，三通道实时视频，三通道长视频，热成像与三通道混合，深度图与三通道混合（今年新推出）。深度图与三通道融合这个新赛道今年只有４个算法参加。

## Evaluation Methodology

If you have experienced with the evaluation protocols, you can skip this part.
如果你已经接触过评价方法，可以跳过这个部分。

The OTB [98] methodology applies a no-reset experiment in which the tracker is initialized in the first frame and it runs unsupervised until the end of the sequence. Performance is summarized by a curve showing the percentage of frames where the overlap of the predicted and the ground truth bounding boxes exceeds a series of predefined thresholds. The area under the plot is the major performance score. A downside of the `AO` is that all frames after the first failure receive a zero overlap, which increases bias and variance of the estimator [52]. Accuracy and robustness are defined as two measures for probing the tracking performance and the expected average overlap `(EAO)` is proposed as the primary measure that combines the two aspects of tracking performance in a principled way.

OTB的方法是无重置的实验，算法只在第一帧进行初始化。表现由预定义的阈值对应重叠度（即预测位置与实际位置的框）的帧数占比画出曲线，曲线下区域即为得分。这种评价方式的问题在于算法在第一次出现错误后就全部得到零重叠，增加了偏好值与不确定性。所以这里采用预期平均重叠作为评价指标。

`VOT` introduced the so-called state-of-the-art bound (`SotA` bound) on all their benchmarks. Any tracker exceeding `SotA` bound is considered state-of-the-art by the `VOT` standard.

在所有标准上都有最好上界，任何表现超过最好上界的算法都被认为是最好的。

The main focus of ST tracking is designing robust trackers that can track throughout significant visual appearance changes without drifting off the target. Whenever a tracker predicts a bounding box not overlapping with the ground truth, a failure is detected and the tracker is re-initialized five frames after the failure. The accuracy is the average overlap between the predicted and ground truth bounding boxes during successful tracking periods. The robustness measures how many times the tracker loses the target (fails) during tracking. The expected average overlap (`EAO`), is an estimator of the average overlap a tracker is expected to attain on a large collection of short-term sequences with the same visual properties as the given dataset.

短视频赛道的主要关注是算法在面对视觉信息发生巨大变化时的鲁棒性。一旦算法的预测位置与实际位置不重叠，则视为失败。连续５帧失败，算法就会被实际位置纠正，重新定义启动。为了消减偏好值，在重启后的１０帧不作为结果。精确度是求的平均重叠度，鲁棒性则是平均失败次数。而`EAO`则是用于预测算法在大量拥有相同特性的数据集上的精确度，简言之，这种方式较好地消除了偏好值和不确定性。

## Dataset

(i) All sequences in the `VOT2018` public dataset were ranked according to their difficulty, using robustness measure averaged over a subset of trackers. Out of 20 least difficult sequences, 12 had been selected for replacement such that the diversity of the dataset has been maintained. 

(ii) Around 150 sequences have been selected at random from the update pool of 1000 sequences collected from the `GOT-10k` dataset.

Challenges: (i) occlusion, (ii) illumination change, (iii) motion change, (iv) size change, (v) camera motion.

比较有意思的是，１９年的数据集把１８年较简单的２０个序列更换了１２个，１５０个序列则是从`GOT-10k`中随机选出。挑战仍然保持了原来的五个：遮挡，光照变化，运动变化，尺寸变化，相机移动。

## Results

Trackers were ranked according to the `EAO` measure on the public dataset. Top five ranked trackers were then re-run by the `VOT2019` committee on the sequestered dataset.

比赛中仍然是将普遍测试中脱颖而出五强算法在隔离数据集上重新测试。

### Short Term 

| Rank |   Name   | Prototype |                           Pipeline                           |
| :--: | :------: | :-------: | :----------------------------------------------------------: |
|  1   |  DRNet   |     -     |  Localization: CNN+DCF+ResNet50/SE; Bounding box regression  |
|  2   | Trackyou |   ATOM    | ATOM+Triplet Loss;<br />Online Updating: fuzzy combination properties |
|  3   |   ATP    |   ATOM    | ATOM+SiamMask for target segmentation; Feature Pyramid Network |

### Real Time

| Rank |    Name    | Prototype |                           Pipeline                           |
| :--: | :--------: | :-------: | :----------------------------------------------------------: |
|  1   | SiamMargin | SiamRPNpp | Offline Training: Standard Siam Framwork; Extract Feature: RoI Align |
|  2   |  SiamFCOT  |     -     | AlexNet+target position: FCOS detector; Unet-like Target Segmentation |

### RGB-Thermal 

| Rank |   Name   | Prototype |                           Pipeline                           |
| :--: | :------: | :-------: | :----------------------------------------------------------: |
|  1   |  JMMAC   |    ECO    | two component: motion cue: key point based+Kalman filer; appearance cue: ECO |
|  2   | SiamDW_T |    ECO    |            JMMAC+Multiple CNNs;<br />Siamese CNNs            |
|  3   |  mfDiMP  |   DiMP    | multi-modal extension of <br />Discriminative Model Prediction |

### RGB-Deep

| Rank |   Name   |   Prototype   |                  Pipeline                   |
| :--: | :------: | :-----------: | :-----------------------------------------: |
|  1   | SiamDW-D |    SiamVGG    | a variant of recent Siamese Network Tracker |
|  2   |  ATCAIS  |     ATOM      |    ATC+HTC instance segmentation method     |
|  3   |  LTDSEd  | ATOM+RT-MDNet |               two components                |





