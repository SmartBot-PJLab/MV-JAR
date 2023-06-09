<div align="center">

<h1>MV-JAR: Masked Voxel Jigsaw and Reconstruction for LiDAR-Based Self-Supervised Pre-Training</h1>

<div>
    <a href='https://scholar.google.com/citations?user=MOobrCcAAAAJ&hl=zh-CN&authuser=1' target='_blank'>Runsen Xu</a>&emsp;
    <a href='https://tai-wang.github.io/' target='_blank'>Tai Wang</a>&emsp;
    <a href='http://zhangwenwei.cn/' target='_blank'>Wenwei Zhang</a>&emsp;
    <a href='https://www.rjchen.site/' target='_blank'>Runjian Chen</a>&emsp;
    <a href='http://www.jinkuncao.com/' target='_blank'>Jinkun Cao</a>&emsp;
    <a href='https://oceanpang.github.io/' target='_blank'>Jiangmiao Pang*</a>&emsp;
    <a href='http://dahua.site/' target='_blank'>Dahua Lin</a>&emsp;
</div>
<a href='https://cvpr2023.thecvf.com/' target='_blank'>CVPR 2023</a>

<a href='https://arxiv.org/abs/2303.13510' target='_blank'>[Paper]</a>  <a href='https://www.youtube.com/watch?v=nlZd-twMOaE' target='_blank'>[Video]</a> <a href='https://drive.google.com/file/d/1wUCKEy-h57z9rBwWE7OypIUFV_S3kWs6/view?usp=sharing' target='_blank'>[Slides]</a> 

</div>

![MV-JAR Overview](https://user-images.githubusercontent.com/82316014/227317603-8f20f4e1-fb0b-4f74-9b40-d25317eb7733.png)

Masked Voxel Jigsaw and Reconstruction (MV-JAR) addresses the uneven distribution of LiDAR points using a Reversed-Furthest-Voxel-Sampling strategy, and combines two techniques for modeling voxel (MVJ) and point (MVR) distributions. We also introduce a new data-efficient 3D object detection benchmark on the Waymo dataset for more accurate evaluation of pre-training methods. Experiments demonstrate that MV-JAR significantly improves 3D detection performance across various data scales. &#x1F4A5;

## News
- [2023-03] We've released the original Waymo filenames of our benchmark sequences to support various codebases.  &#x1F4CA;
- [2023-03] Our data-efficient benchmark on Waymo is released. &#x1F4CA;
- [2023-03] Our paper is accepted by CVPR 2023. &#x1F389;

## Data-Efficient Benchmark on Waymo
### Download
- To download our benchmark information files. You should first register in [Waymo Open Dataset](https://waymo.com/open/) and agree to the [Terms of Use](https://waymo.com/open/terms/). By downloding our benchmark, you agree to the [Terms of Use](https://waymo.com/open/terms/) of Waymo Open Dataset.
- Download our benchmark with [[Google Drive]](https://drive.google.com/drive/folders/1AwDDBSGtTg_ZF2_P1J3wUj1hKrIjZ4co?usp=sharing) or [[Baidu Drive (code: phwq)]](https://pan.baidu.com/s/1jd2ilz6Y5ld8TKWRLbgQRQ?pwd=phwq). They take about 3.2GB in total.

### Usage
#### MMDetection3D Format
- Download and unzip the file `data_efficient_benchmark.zip` and it should be:
  ```
    data_efficient_benchmark
    ├── md5sum.txt
    ├── waymo_dbinfos_train_old_r_0.05_0.pkl
    ├── waymo_dbinfos_train_old_r_0.05_1.pkl
    ├── waymo_dbinfos_train_old_r_0.05_2.pkl
    ├── waymo_dbinfos_train_old_r_0.1_0.pkl
    ├── waymo_dbinfos_train_old_r_0.1_1.pkl
    ├── waymo_dbinfos_train_old_r_0.1_2.pkl
    ├── waymo_dbinfos_train_old_r_0.2_0.pkl
    ├── waymo_dbinfos_train_old_r_0.2_1.pkl
    ├── waymo_dbinfos_train_old_r_0.2_2.pkl
    ├── waymo_dbinfos_train_old_r_0.5_0.pkl
    ├── waymo_dbinfos_train_old_r_0.5_1.pkl
    ├── waymo_dbinfos_train_old_r_0.5_2.pkl
    ├── waymo_infos_train_r_0.05_0.pkl
    ├── waymo_infos_train_r_0.05_1.pkl
    ├── waymo_infos_train_r_0.05_2.pkl
    ├── waymo_infos_train_r_0.1_0.pkl
    ├── waymo_infos_train_r_0.1_1.pkl
    ├── waymo_infos_train_r_0.1_2.pkl
    ├── waymo_infos_train_r_0.2_0.pkl
    ├── waymo_infos_train_r_0.2_1.pkl
    ├── waymo_infos_train_r_0.2_2.pkl
    ├── waymo_infos_train_r_0.5_0.pkl
    ├── waymo_infos_train_r_0.5_1.pkl
    └── waymo_infos_train_r_0.5_2.pkl
  ```
- To check the completeness of the files, you can run the following command:
  ```
  md5sum -c md5sum.txt
  ```

The information files provided in `data_efficient_benchmark/*` are processed according to [MMDetection3D](https://github.com/open-mmlab/mmdetection3d)'s data format, containing only annotations and object point clouds. Before using our benchmark, you must first download the original Waymo dataset and follow MMDetection3D's instructions to convert the data format to MMDetection3D's format. For more details, please refer to MMDetection3D's [Prepare Waymo Dataset](https://mmdetection3d.readthedocs.io/en/v0.18.1/datasets/waymo_det.html) (v1.2).

To use our subsets, simply replace the `waymo_infos_train.pkl` and `waymo_dbinfos_train.pkl` with the corresponding files from our benchmark. For example, to use subset 0 with 5% of the scenes, replace them with `waymo_infos_train_r_0.05_0.pkl` and `waymo_dbinfos_train_old_r_0.05_0.pkl`, respectively.

Here are some notes:
- `waymo_infos_train_r_{data_ratio}_{subset_id}.pkl` files are the annotation files of our sampled subsets. `data_ratio` represents the ratio of the scenes, and `subset_id` is the id of the subset. For example, `waymo_infos_train_r_0.05_0.pkl` is the annotation file of the subset with 5% of the scenes, and its id is 0. For each data ratio, we provide 3 subsets to reduce experimental variance.
- Each larger subset contains smaller subsets with the same subset ID. For example, `waymo_infos_train_r_0.5_0.pkl` contains the scenes in `waymo_infos_train_r_0.2_0.pkl`, which in turn contains the scenes in `waymo_infos_train_r_0.1_0.pkl`, and so on.
- `waymo_dbinfos_train_old_r_{data_ratio}_{subset_id}.pkl` files contain the object point clouds for their corresponding scenes. For example, `waymo_dbinfos_train_old_r_0.05_0.pkl` contains the object point clouds of the scenes listed in `waymo_infos_train_r_0.05_0.pkl`. These `dbinfos` are mainly used for data augmentation.
- &#x2757; Please note that MMDetection3D refactored its coordinate definition beginning from v1.0.0. Our `dbinfos` are based on the old coordinate definition which is why the file names contain `old`. If you use the new coordinate definition, you should generate new `dbinfos` for yourself. For more details, refer to [MMDetection3D's coordinate system refactoring](https://github.com/open-mmlab/mmdetection3d/blob/v1.0.0.dev0/docs/en/compatibility.md#coordinate-system-refactoring).

#### Other Formats
- Download and unzip the file `sequence_names.zip` and it shoulde be:
  ```
    sequence_names
    ├── waymo_infos_train_r_0.05_0_sequence_names.txt
    ├── waymo_infos_train_r_0.05_1_sequence_names.txt
    ├── waymo_infos_train_r_0.05_2_sequence_names.txt
    ├── waymo_infos_train_r_0.1_0_sequence_names.txt
    ├── waymo_infos_train_r_0.1_1_sequence_names.txt
    ├── waymo_infos_train_r_0.1_2_sequence_names.txt
    ├── waymo_infos_train_r_0.2_0_sequence_names.txt
    ├── waymo_infos_train_r_0.2_1_sequence_names.txt
    ├── waymo_infos_train_r_0.2_2_sequence_names.txt
    ├── waymo_infos_train_r_0.5_0_sequence_names.txt
    ├── waymo_infos_train_r_0.5_1_sequence_names.txt
    └── waymo_infos_train_r_0.5_2_sequence_names.txt
  ```
- Each file contains the original Waymo filenames of the sequences in each subsets. For example, `waymo_infos_train_r_0.05_0_sequence_names.txt` contains:
  ```
    segment-5576800480528461086_1000_000_1020_000_with_camera_labels.tfrecord
    segment-11060291335850384275_3761_210_3781_210_with_camera_labels.tfrecord
    ...
  ```
  
You can convert the original Waymo data to any data format for different codebases, such as [OpenPCDet](https://github.com/open-mmlab/OpenPCDet), based on the provided filenames.

## MV-JAR
We are cleaning the code and will release it soon. Stay tuned! &#x1F4E3;

## Citation
If you find our work useful in your research, please cite:
```
@InProceedings{Xu_2023_CVPR,
    author    = {Xu, Runsen and Wang, Tai and Zhang, Wenwei and Chen, Runjian and Cao, Jinkun and Pang, Jiangmiao and Lin, Dahua},
    title     = {MV-JAR: Masked Voxel Jigsaw and Reconstruction for LiDAR-Based Self-Supervised Pre-Training},
    booktitle = {Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition (CVPR)},
    month     = {June},
    year      = {2023},
    pages     = {13445-13454}
}
```

## Acknowledgements
We thank the contributors of [MMDetection3D](https://github.com/open-mmlab/mmdetection3d) and the authors of [SST](https://github.com/tusen-ai/SST) for their great work!
