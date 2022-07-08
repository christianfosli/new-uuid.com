# new-uuid.com

Have you ever wanted to generate a uuid
([universally unique identifier](https://en.m.wikipedia.org/wiki/Universally_unique_identifier)),
without leaving the terminal, and without installing any tools?

Now you can :smile:

```sh
# print to terminal
curl https://new-uuid.com

# copy to clipboard (Windows git-bash or WSL)
curl https://new-uuid.com | clip.exe

# copy to clipboard (Linux with Wayland)
curl https://new-uuid.com | wl-copy

# copy to clipboard (Linux with xorg)
curl https://new-uuid.com | xclip
```

## Development

I'm using Rust to create a web assembly (wasm32-wasi) binary,
and running it on Cloudflare's serverless platform; Cloudflare workers.

This is inspired by [a blogpost by Cloudflare](https://blog.cloudflare.com/announcing-wasi-on-workers).

### Build web assembly binary

**Prerequisites**: Install rust. Add the wasm32-wasi target with `rustup target add wasm32-wasi`.

```sh
cargo build --target wasm32-wasi --release
```

### Run locally with a wasm runtime

**Prerequisites**: Install a wasm runtime, e.g. wasmtime. Build a wasm binary (see above).

```sh
wasmtime ./target/wasm32-wasi/releases/new-uuid.wasm
```

### Run on cloudflare worker for local development

**Prerequisites**: Install node/npm. Create a cloudflare account. Build a wasm binary (see above).

```sh
npx wrangler@wasm dev target/wasm32-wasi/release/new-uuid.wasm

# then test it with curl
curl http://localhost:8787
```

### Deployment

Deployment to Cloudflare is automatic on push to main.
See GitHub actions workflow for details.

<details>
<summary>Manual one-time steps</summary>
<ul>
<li>Buy new-uuid.com domain</li>
<li>Add new-uuid.com as a website in Cloudflare with free plan</li>
<li>Follow the suggested steps for DNS configuration and updating the
  domain registrar to use Cloudflare's name servers</li>
<li>Generate API token in Cloudflare</li>
<li>Add the API token and Account ID to GitHub actions secrets</li>
<li>Add custom domain for worker in Cloudfare dashboard settings</li>
</ul>
</details>
