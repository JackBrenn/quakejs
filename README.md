<div align="center">

# QuakeJS Rootless Project

A fork of QuakeJS with updated dependencies providing the core Quake 3 Arena functionality for the quakejs-rootless project.

[![GitHub](https://img.shields.io/badge/GitHub-JackBrenn%2Fquakejs--rootless-blue?style=for-the-badge&logo=github)](https://github.com/JackBrenn/quakejs-rootless)
[![Docker Hub](https://img.shields.io/badge/Docker%20Hub-awakenedpower%2Fquakejs--rootless-blue?style=for-the-badge&logo=docker)](https://hub.docker.com/r/awakenedpower/quakejs-rootless)

</div>

## 🎮 About

QuakeJS Rootless is a security-focused fork of [nerosketch/quakejs](https://github.com/nerosketch/quakejs), which is a JavaScript port of [ioquake3](http://www.ioquake3.org) using [Emscripten](http://github.com/kripken/emscripten). This project enables you to run Quake 3 Arena in your web browser with enhanced security and modern dependency management.

### 🔒 Security Improvements

This fork dramatically improves the security posture of QuakeJS by updating vulnerable NPM dependencies:

| Severity | Before | After | Improvement |
|----------|--------|-------|-------------|
| CRITICAL | 5 | 0 | ✅ 100% |
| HIGH | 12 | 0 | ✅ 100% |
| MEDIUM | 13 | 1 | ✅ 92% |
| LOW | 52 | 45 | ✅ 13% |

**Key changes:**
- 🔒 Updated all NPM dependencies to remove critical and high-severity vulnerabilities (except "wrench")
- ✨ Refactored code to support modern dependency versions
- 🐳 Rootless Docker support for improved container security

### ⚠️ Security Considerations

While NPM dependencies and code have been updated to modern standards, not all dependencies could be upgraded to the latest versions without a major overhaul of the backend code. **QuakeJS should be considered insecure due to its aging codebase.**

**Recommendations:**
- 🔒 Run in an isolated environment (Rootles Podman container recommended - See link at the top of this README)
- 🌐 Do not expose directly to the internet without additional security measures
- 🛡️ Use behind a reverse proxy with rate limiting
- 📊 Monitor for unusual activity if running publicly

This fork significantly reduces known vulnerabilities but cannot guarantee complete security given the age of the underlying codebase.

### 🚧 Roadmap

- [ ] Migrate to a recent websocket (ws) npm dependency
- [ ] Migrate to fs-extra from wrench (wrench is deprecated)
- [ ] Modernize JavaScript code to ES6+ standards (When I'm a pensjoner!)
- [ ] Improve documentation and examples

## 🚀 Quick Start

### Using Podman (Recommended)

```bash
podman run -d \
  --name quakejs \
  -e HTTP_PORT=8080 \
  -p 8080:8080 \
  -p 27960:27960 \
  docker.io/awakenedpower/quakejs-rootless:latest
```

### Using Docker Run

```bash
docker run -d \
  --name quakejs \
  -e HTTP_PORT=8080 \
  -p 8080:8080 \
  -p 27960:27960 \
  docker.io/awakenedpower/quakejs-rootless:latest
```

Then open your browser and navigate to `http://localhost:8080` to start playing!

### Manual Installation

**Prerequisites:**
- Node.js (v14 or higher recommended)
- npm

**Installation:**

```bash
# Clone the repository
git clone https://github.com/JackBrenn/quakejs-rootless.git
cd quakejs-rootless

# Initialize submodules
git submodule update --init

# Install dependencies
npm install

# Configure content server (using public CDN)
echo '{ "content": "content.quakejs.com" }' > bin/web.json

# Start the server
node bin/web.js --config ./web.json
```

Your server will be available at [http://localhost:8080](http://localhost:8080)

## 🎯 Running a Dedicated Server

### Basic Dedicated Server

For a publicly listed dedicated server:

```bash
node build/ioq3ded.js +set fs_game baseq3 +set dedicated 2 +exec server.cfg
```

For a private server (not listed on master server):

```bash
node build/ioq3ded.js +set fs_game baseq3 +set dedicated 1 +exec server.cfg
```

### Complete baseq3 Server Setup

**Note:** Initial setup requires ~1GB RAM for downloading game files.

1. **Download base game files:**

```bash
git submodule update --init
npm install
node build/ioq3ded.js +set fs_game baseq3 +set dedicated 2
```

2. **Accept the EULA** by pressing Enter through the agreement and typing `y` when prompted

3. **Wait for downloads to complete**, then press `Ctrl+C` to stop

4. **Create server configuration** at `base/baseq3/server.cfg`:

```cfg
seta sv_hostname "My QuakeJS Server"
seta sv_maxclients 12
seta g_motd "Welcome to my server!"
seta g_quadfactor 3
seta g_gametype 0
seta timelimit 15
seta fraglimit 25
seta g_weaponrespawn 3
seta g_inactivity 3000
seta g_forcerespawn 0
seta rconpassword "CHANGE_ME_SECURE_PASSWORD"
set d1 "map q3dm7 ; set nextmap vstr d2"
set d2 "map q3dm17 ; set nextmap vstr d1"
vstr d1
```

5. **Start your server:**

```bash
node build/ioq3ded.js +set fs_game baseq3 +set dedicated 2 +exec server.cfg
```

6. **Connect** at: `http://www.quakejs.com/play?connect%20YOUR_SERVER_IP:27960`

## 🗂️ Running a Content Server

QuakeJS can use a custom content server for hosting game assets and mods.

### Repackaging Assets

```bash
node bin/repak.js --src <source_pk3_directory> --dest <output_directory>
```

### Starting Content Server

```bash
node bin/content.js
```

### Using Custom Content Server

Launch your game/dedicated server with:

```bash
node bin/web.js +set fs_cdn http://your-content-server:port
```

## 🎨 Adding Custom Maps & Content

For detailed guides on adding custom content:

- **Linux:** See [cctools/README.md](cctools/README.md) by [@digidigital](https://github.com/digidigital/)
- **Windows:** [Step-by-step guide](https://steamforge.net/wiki/index.php/How_to_setup_a_local_QuakeJS_server_under_Windows_10#Adding_your_own_Maps)
- **Video tutorial:** [YouTube guide](https://www.youtube.com/watch?v=m57rMXASWms) by [@grabisoft](https://github.com/grabisoft)

## 🏗️ Building from Source

To build the Emscripten binaries yourself:

**Prerequisites:**
- Working [Emscripten](http://github.com/kripken/emscripten) installation

```bash
cd quakejs/ioq3
make PLATFORM=js EMSCRIPTEN=<path_to_emscripten>
```

Binaries will be output to `ioq3/build/release-js-js/`

## 🌐 Fully Local Setup

For completely offline LAN setups:

1. Set up a dedicated server (see above)
2. Copy `html/*` to your web server root
3. Run `html/get_assets.sh` to download assets from content.quakejs.com
4. Edit line 77 of `html/index.html` to point to your server
5. (Optional) Set up as a system service using `init.d/quakejs`

Detailed guide: [Local QuakeJS Server Setup](https://steamforge.net/wiki/index.php/How_to_setup_a_local_QuakeJS_server_under_Debian_9_or_Debian_10)

## 🔗 Related Projects

- [nerosketch/quakejs](https://github.com/nerosketch/quakejs) - The upstream fork this project is based on
- [quakejs-installer](https://github.com/digidigital/quakejs-installer) - One-step Linux installer with common addons
- [quakejs-docker](https://github.com/treyyoder/quakejs-docker) - Fully local Dockerized QuakeJS server
- [quake-kube](https://github.com/criticalstack/quake-kube) - Kubernetes deployment for QuakeJS
- [begleysm/quakejs](https://github.com/begleysm/quakejs) - Another popular QuakeJS fork with local hosting
- [inolen/quakejs](https://github.com/inolen/quakejs/) - The original QuakeJS project

## 🤝 Contributing

Contributions are welcome! Please feel free to submit pull requests or open issues for:

- Security improvements
- Dependency updates
- Bug fixes
- Documentation improvements
- New features

## 📝 License

MIT License - See [LICENSE](LICENSE) file for details

## 🙏 Credits

- Original QuakeJS port by [@inolen](https://github.com/inolen)
- Upstream fork by [@nerosketch](https://github.com/nerosketch)
- Built on [ioquake3](http://www.ioquake3.org) and [Emscripten](http://github.com/kripken/emscripten)

---

<div align="center">

**[Documentation](https://github.com/JackBrenn/quakejs-rootless/wiki)** • **[Report Bug](https://github.com/JackBrenn/quakejs-rootless/issues)** • **[Request Feature](https://github.com/JackBrenn/quakejs-rootless/issues)**

</div>
