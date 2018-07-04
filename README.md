# js-files-images
图片输入并使用
//首先是在html文件中的段落；
<div id="file-container" style="display: none">
    Upload an image: <input type="file" id="files" name="files[]" multiple />
</div>


//其次是js文件；
const filesElement = document.getElementById('files');
filesElement.addEventListener('change', evt => {
//监听操作；
  let files = evt.target.files;
  // Display thumbnails & issue call to predict each image.
  for (let i = 0, f; f = files[i]; i++) {
    // Only process image files (skip non image files)判断是否为图片；
    if (!f.type.match('image.*')) {
      continue;
    }
    let reader = new FileReader();
    const idx = i;
    // Closure to capture the file information.
    reader.onload = e => {
      // Fill the image & call predict.
      let img = document.createElement('img');
      img.src = e.target.result;
      img.width = IMAGE_SIZE;
      img.height = IMAGE_SIZE;
      img.onload = () => predict(img);
    };

    // Read in the image file as a data URL.
    reader.readAsDataURL(f);
  }
});
