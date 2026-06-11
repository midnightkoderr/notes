# Notes

## init termux

```bash
curl -fsSL https://gist.githubusercontent.com/midnightkoderr/1d1dc5ab4126fadc86ad6e6dba240952/raw/7a2c992f762d71d17b4ceeaed5bfb878b7b4be77/termux_init.sh | bash
```

## install llama-cpp

```bash
#!/data/data/com.termux/files/usr/bin/bash

curl -fsSL https://raw.githubusercontent.com/midnightkoderr/llama.cpp-android-gpu/main/installer.sh | bash -s -- --install-dir ~/.local/bin --lib-dir ~/.local/lib

echo 'export LD_LIBRARY_PATH=${HOME}/.local/lib:${LD_LIBRARY_PATH}' >> ~/.bashrc

echo "Installation complete. Please add ~/.local/bin to your PATH and ~/.local/lib to your LD_LIBRARY_PATH if you haven't already."
```
