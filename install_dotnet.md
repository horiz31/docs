Download and copy to device
For .net5.0 for ARM64 https://download.visualstudio.microsoft.com/download/pr/d0a22fa3-b916-49ce-8284-97131b424cb3/cb884163ad34b83f1ae1dbd33e09d77a/aspnetcore-runtime-5.0.7-linux-arm64.tar.gz


extract to /usr/local/bin/dotnet
```
sudo mkdir -p /usr/local/bin/dotnet 
sudo tar zxf <file downloaded above> -C /usr/local/bin/dotnet
```
set paths in ~/.bashrc

```
sudo nano ~/.bashrc
```
add the following lines to the bottom
```
# dotnet
export DOTNET_ROOT=/usr/local/bin/dotnet
export PATH=$PATH:/usr/local/bin/dotnet
```
reboot  

To verify you have it installed
```
dotnet --list-runtimes
```
you shoud see .NET x.x.x at /usr/local/bin/dotnet/shared/Microsoft.NetCore.App
