FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["DemoAutoUpdateNet6/DemoAutoUpdateNet6.csproj", "DemoAutoUpdateNet6/"]
RUN dotnet restore "DemoAutoUpdateNet6/DemoAutoUpdateNet6.csproj"
COPY . .
WORKDIR "/src/DemoAutoUpdateNet6"
RUN dotnet build "DemoAutoUpdateNet6.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "DemoAutoUpdateNet6.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "DemoAutoUpdateNet6.dll"]
