<div align="center">

# QuakeJS Rootless Project

A fork of QuakeJS with updated dependencies providing the core Quake 3 Arena functionality for the quakejs-rootless project.

[![GitHub](https://img.shields.io/badge/GitHub-JackBrenn%2Fquakejs--rootless-blue?style=for-the-badge&logo=github)](https://github.com/JackBrenn/quakejs-rootless)
[![Docker Hub](https://img.shields.io/badge/Docker%20Hub-awakenedpower%2Fquakejs--rootless-blue?style=for-the-badge&logo=docker)](https://hub.docker.com/r/awakenedpower/quakejs-rootless)

</div>

## About

QuakeJS Rootless is a security-focused fork of [nerosketch/quakejs](https://github.com/nerosketch/quakejs), which is a JavaScript port of [ioquake3](http://www.ioquake3.org) using [Emscripten](http://github.com/kripken/emscripten). This project enables you to run Quake 3 Arena in your web browser with enhanced security and modern dependency management.

### Security Improvements

This fork dramatically improves the security posture of QuakeJS by updating vulnerable NPM packages.

**Key changes:**
- Updated all NPM dependencies to remove critical and high-severity vulnerabilities (except "wrench")
- Refactored code to support modern dependency versions
- Rootless Docker support for improved container securit

## License

MIT License - See [LICENSE](LICENSE) file for details

## 🙏 Credits

- Original QuakeJS port by [@inolen](https://github.com/inolen)
- Upstream fork by [@nerosketch](https://github.com/nerosketch)
- Built on [ioquake3](http://www.ioquake3.org) and [Emscripten](http://github.com/kripken/emscripten)

---

<div align="center">

**[Documentation](https://github.com/JackBrenn/quakejs-rootless/wiki)** • **[Report Bug](https://github.com/JackBrenn/quakejs-rootless/issues)** • **[Request Feature](https://github.com/JackBrenn/quakejs-rootless/issues)**

</div>
