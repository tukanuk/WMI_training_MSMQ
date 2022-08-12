# Example snippets for building and uploading the extension

```zsh
export EXTENSION_TOKEN=<api_token>
export VERSION=1.0.xx
dt extension build --target-directory ./build --private-key "/Users/admin/dev/certs/developer.key" --no-dev-passphrase --certificate "/Users/admin/dev/certs/developer.pem"

dt extension upload --tenant-url https://aaa12345.live.dynatrace.com --api-token $EXTENSION_TOKEN "./build/custom_com.dynatrace.extension.wmi.msmq-$VERSION.zip"
```