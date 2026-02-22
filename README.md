## Port-Forwarding

### Required ports:

TCP:

* 7880-7881 (LiveKit RTC)

UDP:

* 3478 (Turn-Listen)  
* 40000-49999 (Turn-Relay)  
* 50000-60000 (RTC Media)

# 1\. Clone git repository

Run the following commands:  
`git clone https://github.com/fluxerapp/fluxer.git`

`cd fluxer`

# 2\. Setup nix/devenv

### Install Nix

`sh <(curl -L https://nixos.org/nix/install) --daemon`

### Install devenv

`nix-env --install --attr devenv -f https://github.com/NixOS/nixpkgs/tarball/nixpkgs-unstable`

### Enter devenv shell

`devenv shell`

`pnpm install`

# 

# 3\. Configuration

### Copy configuration template

`cp config/config.dev.template.json config/config.json`

### Modify configuration (Using Nano)

See [example.config.json](https://github.com/Zinal001/fluxer-setup/blob/main/example.config.json) for an example file.

`nano config/config.json`

**Important changes:**  
	* Change “**domain**”.”**base\_domain**” from localhost to your domain (i.e. [fluxer.example.com](http://fluxer.example.com)).  
	* Add “**domain**”.”**public\_scheme**”: “https” if you want the app to be secure (Requires an SSL certificate).  
	* Modify “**services**”.”**gateway**”.”**media\_proxy\_endpoint**”, change localhost to your domain.  
	* Modify “**auth**”.”**bluesky**”.”**enabled**” to false. (Disables bluesky entirely)  
	* Modify “**integrations**”.”**voice**”.”**url**”, change localhost to your domain.
  *(Note: If you are using an SSL certificate, change ws:// to wss://.)*  
	* Modify “**integrations**”.”**voice**”.”**webhook\_url**”, change localhost to your domain.

Save configuration using **CTRL \+ O**, **Enter** and then exit Nano using **CTRL \+ X**.

# 4\. Hard-coded modifications

## Some config options doesn’t seem to affect the code as they should

### Modify RuntimeConfigStore.tsx

`nano fluxer_app/src/stores/RuntimeConfigStore.tsx`

Change line 231 from:

`const bootstrapEndpoint = this.apiEndpoint || Config.PUBLIC_BOOTSTRAP_API_ENDPOINT;`

to  

`const bootstrapEndpoint = "https://DOMAIN:48763"; `
(i.e. [https://fluxer.example.com](https://fluxer.example.com))

Save configuration using **CTRL \+ O**, **Enter** and then exit Nano using **CTRL \+ X**.

### Modify VoiceConnectionManager.tsx

`nano fluxer_app/src/stores/voice/VoiceConnectionManager.tsx`

Change line 179 from:  

`const endpoint = raw.endpoint ?? null;`

to  

`const endpoint = "wss://DOMAIN:7880";`
(i.e. [https://fluxer.example.com](https://fluxer.example.com))

Save configuration using **CTRL \+ O**, **Enter** and then exit Nano using **CTRL \+ X**.

# 5\. Run

### Setup Configuration Schema

`pnpm run schema:generate`

### Run

`pnpm run dev`

# 6\. Register account

Goto [https://DOMAIN:48763](https://DOMAIN:48763) and register a new account.  
If you left the SMTP config as default, then goto [https://DOMAIN:48763/mailpit](https://DOMAIN:48763/mailpit) to activate the account.

**Enjoy\!**
