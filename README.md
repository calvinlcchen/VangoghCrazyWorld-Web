# Vangogh Crazy World - Web code

Here is the practice about how to make models that are trained by python code in <a href="https://github.com/acerwebai/VangoghCrazyWorld"> VangoghCrazyWorld</a>.<br/>
It's implemented by ML5.js and P5.js. <br/>
ML5.js is a high end API base on TensorFlow.js that published by Google.
You can get mode details from the <a href="https://ml5js.org/">ML5.js web</a> and <a href="https://www.tensorflow.org/js/">tensorflow.js web </a>
<br/>
Following describes what is the steps that we need to do step by step. Enjoy it.

## Convert models
 After trained your own model by the python code in <a href="https://github.com/acerwebai/VangoghCrazyWorld"> VangoghCrazyWorld</a> that we modified from the code <a href="https://github.com/lengstrom/fast-style-transfer"> published by lengstrom</a>.<br/>
 git clone the code from <a href="https://github.com/reiinakano/fast-style-transfer-deeplearnjs"> fast-style-transfer-deeplearnjs</a><br/>
 Following below instructions to convert TensorFlow checkpoint files to ML5 supported variable files.
 <pre>
 $ python scripts/dump_checkpoint_vars.py --output_dir=src/ckpts/my-new-style --checkpoint_file=/path/to/model.ckpt
 $ python scripts/remove_optimizer_variables.py --output_dir=src/ckpts/my-new-style
 </pre>
## Inference performance adjustment
To get a more efficent inference time on web experience. to reduce the model size is necessary. <br/>
We modify the num_filter when we training tensorflow models. To get the enough quality and acceptable inference performance.<br/>
We do some experiments, and get following benchmark table.
<table>
 <tr>
  <td></td><td>UHD 620 (250x250) </td><td>UHD 620 (480X480)</td></tr>
 <tr><td> 4 num filter</td><td></td><td></td>
 </tr>
 </table>
As the benchmark table, we think the result of 8 num_filter and the input image with 480x480 dimension is acceptable. <br/>
No matter inference time per frame or output quality on web experience.
Therefore, we adjust the initial num filter to 8 from 32 that default in python code of <a href="https://github.com/lengstrom/fast-style-transfer"> published by lengstrom</a> for training.
<br/>
## ML5.js library adjustment
The original ML5.js library is limit the initial num filter as 32. To support our modified models, we need revise the ml5.min.js library.
Here is the change we did in the library.<br/>
1. input dimension for video: 200x200 -> 480x480<br/>
modify index.js of StyleTransfer in ML5.js library
<pre>
const IMAGE_SIZE = 200;
</pre>
to 
<pre>
const IMAGE_SIZE = 480;
</pre>
2. fulfill initial num filter modification of trained models.<br/>
modify index.js of StyleTransfer in ML5.js library
<pre>
      const convT1 = this.convTransposeLayer(res5, 64, 2, 39);
      const convT2 = this.convTransposeLayer(convT1, 32, 2, 42);
</pre>
to
<pre>
      const convT1 = this.convTransposeLayer(res5, 16, 2, 39);
      const convT2 = this.convTransposeLayer(convT1, 8, 2, 42);
</pre>

## Credits 
#### The authers of <a href="https://github.com/reiinakano/fast-style-transfer-deeplearnjs"> fast-style-transfer-deeplearnjs</a>
#### The authers of <a href="https://github.com/yining1023/fast_style_transfer_in_ML5/"> fast-style transfer-in-ML5</a>
#### The author of <a href="https://github.com/lengstrom/fast-style-transfer"> fast-style-transfer by tensorflow</a>
#### <a href="https://github.com/ml5js/ml5-library"> ML5.js Library github</a>

## License

This project is licensed under the MIT, see the [LICENSE.md](LICENSE)

## S

* Hat tip to anyone whose code was used
* Inspiration
* ...


