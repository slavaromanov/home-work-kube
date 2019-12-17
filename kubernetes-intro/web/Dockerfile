FROM haskell:8

# compile file server
RUN cabal update
RUN cabal install wai-app-static
RUN cp /root/.cabal/bin/warp /bin/warp

# create user with UID 1001
RUN useradd -r -u 1001 appuser

# create /app dir
WORKDIR /app
# grant privelege to UID 1001
RUN chown appuser /app

# set UID to 1001
USER appuser

ENTRYPOINT ["/bin/warp", "-p", "8000"]
