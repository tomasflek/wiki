# Other notes


## Create .NET console application form cmd
> Make sure that [.NET SDK](https://dotnet.microsoft.com/en-us/download) is installed. 
```
dotnet new console -o myapp
```

## Linux on Windows (WSL)

###  [Install WSL](https://docs.microsoft.com/en-us/windows/wsl/install)

### Ping to www.google.com does not work

ping: www.google.com: Temporary failure in name resolution

To solve the issue edit `nameserver` to `8.8.8.8`.  
> sudo vi /etc/resolv.conf

### [Install the .NET SDK on Ubuntu](https://docs.microsoft.com/en-us/dotnet/core/install/linux-ubuntu)

```
sudo apt-get update; \
  sudo apt-get install -y apt-transport-https && \
  sudo apt-get update && \
  sudo apt-get install -y dotnet-sdk-6.0
```
if commend above ends up with error *Unable to locate package dotnet-sdk-6.0* proceed with following command:

```
mkdir ~/.dotnet 
cd ~/.dotnet
wget https://download.visualstudio.microsoft.com/download/pr/17b6759f-1af0-41bc-ab12-209ba0377779/e8d02195dbf1434b940e0f05ae086453/dotnet-sdk-6.0.100-linux-x64.tar.gz
tar -xf dotnet-sdk-6.0.100-linux-x64.tar.gz
export PATH="$PATH:$HOME/.dotnet"
rm -rf dotnet-sdk-6.0.100-linux-x64.tar.gz
```
### Access Windows folders from Linux

```
cd /mnt/c/SourceCode/new-connector/ConnectorConsole/bin/Debug/net6.0
```