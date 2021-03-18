# Scorpio
Pixel Transparency to Replace the Background - All but white using canvas
One of the hallmarks of Flash video, before the arrival of HTML5 video, was the ability to use alpha channel video on top of an animation or a static image. This technique can't be used in the HTML5 universe, but manipulation of the canvas allows us to determine which colors are transparent and to overlay that canvas over other content. Listing 5-8 shows a video where all colors but white are made transparent before being projected onto a canvas with a background image. In the browser, pixels consist of a combination of three colors: red, green, and blue. Each of the r, g, and b components can have a value between 0 and 255 equating to 0% to 100% intensity. Black is when all rgb values are 0 and white when all of them are 1. In Listing 5-8, we identify a pixel whose r, g, and b components are all above 180 as close enough to white so we can keep some more "dirty" whites also.

Listing 5-8:
function paintFrame() {
  w = 360; h = 240;
  context.drawImage(video, 0, 0, w, h);
  frame = context.getImageData(0, 0, w, h);
  context.clearRect(0, 0, w, h);
  output = context.createImageData(w, h);

  // Loop over each pixel of output file
  for (x = 0; x < w; x++) {
    for (y = 0; y < h; y++) {
      // index in output image
      i = x + w*y;
      for (c = 0; c < 4; c++) {
        output.data[4*i+c] = frame.data[4*i+c];
      }
      // make pixels transparent
      r = frame.data[i * 4 + 0];
      g = frame.data[i * 4 + 1];
      b = frame.data[i * 4 + 2];
      if (!(r > 180 && g > 180 && b > 180))
        output.data[4*i + 3] = 0;
    }
  }
  context.putImageData(output, 0, 0);
  if (video.paused || video.ended) {
    return;
  }
  requestAnimationFrame(paintFrame);
}

Listing 5-8 shows the essential painting function. The rest of the page is very similar to Listing 5-2 with the addition of a background image to the <canvas> styling. All pixels are drawn in exactly the same manner, except for the fourth color channel of each pixel, which is set to 0. This is the a channel, which determines opacity in the rbga color model, so we're making all pixels opaque that are not white. 
