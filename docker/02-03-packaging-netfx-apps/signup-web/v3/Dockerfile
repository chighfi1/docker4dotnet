# escape=`
#This v3 file makes better use of what docker can do
# Same as batch file, but by explicitly moving them into my docker file then ordering
# them to the things that change least come firlst, they will come from cache as much as
# possible
# and the things that change more frequently are at the end of the builds that will run
# every time a source file has changed
FROM mcr.microsoft.com/dotnet/core/sdk:3.1-alpine AS builder

#Rather than copying the whole source directory, it just copies in
#the individual files that describe the dependencies for the application
WORKDIR C:\src
COPY src\SignUp.Web.sln .
COPY src\SignUp.Core\SignUp.Core.csproj .\SignUp.Core\
COPY src\SignUp.Entities\SignUp.Entities.csproj .\SignUp.Entities\
COPY src\SignUp.Messaging\SignUp.Messaging.csproj .\SignUp.Messaging\
COPY src\SignUp.Model\SignUp.Model.csproj .\SignUp.Model\
COPY src\SignUp.Web\SignUp.Web.csproj .\SignUp.Web\
# for my .net framework dependencies, I'm copying the packages, not config files as well
# When I copied in all the files that describe the dependencies, then I'm running the nuget
# restore inside the docker file
# This means that if I don't change any of my apps dependencies between builds, then this 
# I don't need a nuget restore every time
COPY src\SignUp.Model\packages.config .\SignUp.Model\
COPY src\SignUp.Web\packages.config .\SignUp.Web\
RUN nuget restore SignUp.Web.sln

# Copies the whole rest of the source folder and runs the MSBuild command
# To package up the application
COPY src C:\src
RUN msbuild SignUp.Web\SignUp.Web.csproj /p:OutputPath=c:\out /p:Configuration=Release

# app image
FROM mcr.microsoft.com/dotnet/core/sdk:3.1-alpine
SHELL ["powershell", "-Command", "$ErrorActionPreference = 'Stop';"]

ENV APP_ROOT=C:\web-app

WORKDIR ${APP_ROOT}
RUN New-WebApplication -Name 'app' -Site 'Default Web Site' -PhysicalPath $env:APP_ROOT

# COpy from the builder stage, I know where the source is going to be
# my C:/out/PUblisheWebsites folder, then I copy it to the APP_ROOT folder
COPY --from=builder C:\out\_PublishedWebsites\SignUp.Web ${APP_ROOT}