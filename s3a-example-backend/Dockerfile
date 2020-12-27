#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:5.0-buster-slim AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:5.0-buster-slim AS build
WORKDIR /src
COPY ["s3a-example-backend/s3a-example-backend.csproj", "s3a-example-backend/"]
RUN dotnet restore "s3a-example-backend/s3a-example-backend.csproj"
COPY . .
WORKDIR "/src/s3a-example-backend"
RUN dotnet build "s3a-example-backend.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "s3a-example-backend.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "s3a-example-backend.dll"]