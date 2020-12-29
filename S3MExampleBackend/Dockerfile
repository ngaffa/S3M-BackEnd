#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/core/aspnet:3.1-buster-slim AS base
WORKDIR /app
EXPOSE 80

FROM mcr.microsoft.com/dotnet/core/sdk:3.1-buster AS build
WORKDIR /src
COPY ["S3MExampleBackend/S3MExampleBackend.csproj", "S3MExampleBackend/"]
RUN dotnet restore "S3MExampleBackend/S3MExampleBackend.csproj"
COPY . .
WORKDIR "/src/S3MExampleBackend"
RUN dotnet build "S3MExampleBackend.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "S3MExampleBackend.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "S3MExampleBackend.dll"]