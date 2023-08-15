VERSION 0.7
FROM mcr.microsoft.com/dotnet/aspnet:6.0 
WORKDIR /dotnet-example

deps:
    # copying project files and restoring NuGet packages allows docker to cache the layer and only re-build it when NuGet packages change
    COPY src/MiniCore/MiniCore.csproj src/MiniCore/
    RUN dotnet restore src/MiniCore

build:
    FROM +deps
    COPY src src

    # make sure you have /bin and /obj in .earthlyignore, as their content from context might cause problems
    RUN dotnet publish --no-restore src/MiniCore -o publish

    SAVE ARTIFACT publish AS LOCAL publish

docker:
    FROM mcr.microsoft.com/dotnet/aspnet:6.0 
    COPY +build/publish .
    ENTRYPOINT ["dotnet", "MiniCore.dll"]
    SAVE IMAGE --push welovetwn/minicore:latest
