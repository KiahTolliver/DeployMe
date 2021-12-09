#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY ["DeployMe/Server/DeployMe.Server.csproj", "DeployMe/Server/"]
COPY ["DeployMe/Client/DeployMe.Client.csproj", "DeployMe/Client/"]
COPY ["DeployMe/Shared/DeployMe.Shared.csproj", "DeployMe/Shared/"]
RUN dotnet restore "DeployMe/Server/DeployMe.Server.csproj"
COPY . .
WORKDIR "/src/DeployMe/Server"
RUN dotnet build "DeployMe.Server.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "DeployMe.Server.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "DeployMe.Server.dll"]
