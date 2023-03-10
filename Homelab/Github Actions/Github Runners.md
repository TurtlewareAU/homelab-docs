
```bash
`# Create a folder   $ mkdir actions-runner && cd actions-runner` `# Download the latest runner package   $ curl -o actions-runner-linux-x64-2.302.1.tar.gz -L https://github.com/actions/runner/releases/download/v2.302.1/actions-runner-linux-x64-2.302.1.tar.gz # Optional: Validate the hash   $ echo "3d357d4da3449a3b2c644dee1cc245436c09b6e5ece3e26a05bb3025010ea14d actions-runner-linux-x64-2.302.1.tar.gz" | shasum -a 256 -c # Extract the installer   $ tar xzf ./actions-runner-linux-x64-2.302.1.tar.gz`
```

```bash
`# Create the runner and start the configuration experience   $ ./config.sh --url https://github.com/TurtlewareAU --token ACCRMS6EGX7FA5WIAFDW2E3EBKFPG # Last step, run it!   $ ./run.sh`
```