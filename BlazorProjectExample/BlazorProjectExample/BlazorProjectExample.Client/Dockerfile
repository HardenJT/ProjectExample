FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
USER $APP_UID
WORKDIR /app
EXPOSE 8080
EXPOSE 8081

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["BlazorProjectExample/BlazorProjectExample.Client/BlazorProjectExample.Client.csproj", "BlazorProjectExample/BlazorProjectExample.Client/"]
RUN dotnet restore "BlazorProjectExample/BlazorProjectExample.Client/BlazorProjectExample.Client.csproj"
COPY . .
WORKDIR "/src/BlazorProjectExample/BlazorProjectExample.Client"
RUN dotnet build "BlazorProjectExample.Client.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "BlazorProjectExample.Client.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "BlazorProjectExample.Client.dll"]
