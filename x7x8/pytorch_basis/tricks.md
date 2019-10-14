# some notes from mmdet

## multiscale

- The term of "multiscale training" is adopted in many papers, which indicates resizing images to different scales at each iteration. 
- In the challenge, we use the setting [(400, 1600), (1400, 1600)] which means the short edge are randomly sampled from 400~1400, and the long edge is fixed as 1600.