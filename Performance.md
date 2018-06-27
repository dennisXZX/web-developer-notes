## Performance

You can check the performance benchmark using [PageSpeed Insight](https://developers.google.com/speed/pagespeed/insights/) and [WebPagetest](https://www.webpagetest.org/).

Following is a list of things you can do to optimize your web app.

- Use [minifier](https://www.minifier.org/) to minimize JS and CSS code

- Minimize images
  - Use PNG for transparency, GIF for animation, JPEG for colorful images
  - Use [TinyPNG](https://tinypng.com/) and [JPEG optimizer](http://jpeg-optimizer.com/) for compressing images
  - Lower JPEG image quality to about 30% - 60% ([ImageOptim](https://imageoptim.com/))
  - Resize image based on the size it will be displayed on the screen
  - Display different sized images for different screen sizes

    ```
    @media screen and (min-width: 500px) {
      body {
        background: url('./medium-bg.jpg') no-repeat center center fixed;
        background-size: cover;
      }
    }

    @media screen and (min-width: 900px) {
      body {
        background: url('./large-bg.jpg') no-repeat center center fixed;
        background-size: cover;
      }
    }
    ```

  - Use images CDNs such as [imigx](https://www.imgix.com/)
  - Remove metadata of images using [verexif](http://www.verexif.com/)

- Reduce the number of files that need to be fetched using bundling technique

- Use above the fold CSS loading, separate the above-the-fold CSS style into a file and load it first

- Load style tag in the <head> and script right before </body>

- Use `<script async>` to load JS file asynchronously. These scripts should not modify DOM, such as Google Analytics script, because there is no guarantee when they will be loaded. Use `<script defer>` to load scripts that are not required above the fold.

- Implement route-based code splitting, which utilizes `dynamic import()` under the hood to achieve the splitting.

- Avoid unnecessary rendering, for example, we can consider using `PureComponent` and implementing `shouldComponentUpdate` life cycle method in React to fine tune component rendering.
