FROM rubensa/ubuntu-tini-dev-chrome

# Keep container running (for use in VSCode)
CMD [ "tail", "-f", "/dev/null" ]

# Allow vscode-server cache
RUN mkdir -p /home/${USER_NAME}/.vscode-server

# Configure local node modules
RUN printf "\nexport PATH=\"\$PWD/node_modules/.bin:\$PATH\"\n" >> /home/${USER_NAME}/.bashrc
