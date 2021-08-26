<!-- PROJECT LOGO -->
<p align="center">
  <a href="https://dyte.in">
    <img src="https://dyte-uploads.s3.ap-south-1.amazonaws.com/dyte-logo-dark.svg" alt="Logo" width="80">
  </a>

  <h3 align="center">Web Core Integration Sample</h3>

  <p align="center">
    Examples on integrating Dyte Web Core with web applications.
    <br />
    <a href="https://docs.dyte.in"><strong>Explore the docs »</strong></a>
    <br />
    <br />
    <a href="https://app.dyte.in">View Demo</a>
    ·
    <a href="https://docs.dyte.in/discuss">Report Bug</a>
    ·
    <a href="https://docs.dyte.in/discuss">Request Feature</a>
  </p>
</p>




<!-- TABLE OF CONTENTS -->
## Table of Contents

* [About the Project](#about-the-project)
  * [Built With](#built-with)
* [Getting Started](#getting-started)
  * [Prerequisites](#prerequisites)
  * [Installation](#installation)
* [Usage](#usage)
* [Roadmap](#roadmap)
* [Contributing](#contributing)
* [License](#license)
* [Contributors](#contributors-)



<!-- ABOUT THE PROJECT -->
## About The Project

This repository consists of sample apps for demonstrating how to integrate [@dytesdk/web-core](https://www.npmjs.com/package/@dytesdk/web-core) with web applications.


### Built With

* [Dyte Web Core](https://www.npmjs.com/package/@dytesdk/web-core)


<!-- GETTING STARTED -->
## Getting Started

To run the demo website, the `index.html` file must be served. One way this can be done is using a Python simple HTTP server.

```bash
python3 -m http.server 3000
```

Now, the demo will be visible on http://localhost:3000.

### Installation
 
1. Clone the repo
```sh
git clone https://github.com/dyte-in/docs-template.git
```

2. Serve the index.html file.
```sh
python3 -m http.server 3000
```


<!-- USAGE EXAMPLES -->
## Usage

The Dyte Web Core SDK can be installed using npm, or can be set up using a CDN.

### Using npm
```sh
npm install @dytesdk/web-core
```

### Using the CDN
```html
<script src="https://cdn.dyte.in/core/dyte-0.0.19.js"></script>
```
> Note: The CDN file is named in the following manner: `dyte-${version}.js`. To get the latest version (which is subject to breaking changes), you may use https://cdn.dyte.in/core/dyte.js.

_Please refer to the [Documentation](./docs/api.md)._



<!-- ROADMAP -->
## Roadmap

See the [open issues](https://github.com/dyte-in/docs-template/issues) for a list of proposed features (and known issues).



<!-- CONTRIBUTING -->
## Contributing

Contributions are what make the open source community such an amazing place to be learn, inspire, and create. Any contributions you make are **greatly appreciated**.

1. Fork the Project
2. Create your Feature Branch (`git checkout -b feature/AmazingFeature`)
3. Commit your Changes (`git commit -m 'feat: Add some AmazingFeature'`)
4. Push to the Branch (`git push -u origin feature/AmazingFeature`)
5. Open a Pull Request

You are requested to follow the contribution guidelines specified in [CONTRIBUTING.md](./CONTRIBUTING.md) while contributing to the project :smile:.

<!-- LICENSE -->
## License

Distributed under the MIT License. See [`LICENSE`](./LICENSE) for more information.




<!-- MARKDOWN LINKS & IMAGES -->
<!-- https://www.markdownguide.org/basic-syntax/#reference-style-links -->
