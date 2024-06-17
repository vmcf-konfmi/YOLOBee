# Notes

## Files
 - [ c ] create shared google drive
 - [ ] annotation folder
 - [ ] publish training data?

## Plan
 - Test model training
 - Cut videos per frames (openCV)
 - Create small annotations (annotation_model - ease up annotations in makesense.ai)
   - make a simple function to divide images and annotations to train and test folders (70/30 %)
 - Set up training and validations set of videos
 - Create the "big" training set (with use of annotation_model)
 - Train s (small) model with YOLOv5 (either Collab, mazlik or metacentrum server)
 - Decide what to compare (YOLOv5 vs YOLOv8 or model sizes)

### Short term
- testing github
- YOLOv5 testing
  - example use-case [video](https://youtu.be/gDoMYuyY_qw?si=-DlDqhvWlfU2M6Ac)
  - Ultralytics [YOLOv5 GitHub](https://github.com/ultralytics/yolov5)
  - Explore [SciCount](https://github.com/martinschatz-cz/SciCount) - maybe start with
- Cut videos per frames
- Create a small image subset (30 images max, something like every 30th frame)
- Use [makesnes.ai](https://www.makesense.ai/) for annotations [video](https://www.loom.com/share/4d6ca48b639a4fd1a17ade04c73d935e?sid=16615971-76c9-4b12-9e96-19b169c64d53)


#### Medium term
- compare yolov5s, yolov5m, yolov5x
- do the clagh augmetation
   - retrain the network with the same dataset
   - compare again the different yolo version
- trace on frames the center on the bounding boxes and keep the coordinates of thoses center on a text files 
