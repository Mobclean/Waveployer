FROM mcr.microsoft.com/dotnet/runtime:8.0 AS base
USER $APP_UID
WORKDIR /app

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["Waveployer.Crawler/Waveployer.Crawler.csproj", "Waveployer.Crawler/"]
RUN dotnet restore "Waveployer.Crawler/Waveployer.Crawler.csproj"
COPY . .
WORKDIR "/src/Waveployer.Crawler"
RUN dotnet build "Waveployer.Crawler.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "Waveployer.Crawler.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Waveployer.Crawler.dll"]
