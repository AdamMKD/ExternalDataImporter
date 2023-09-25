FROM mcr.microsoft.com/dotnet/aspnet:7.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:7.0 AS build
WORKDIR /src
COPY ["ExternalDataImporter/ExternalDataImporter.csproj", "ExternalDataImporter/"]
RUN dotnet restore "ExternalDataImporter/ExternalDataImporter.csproj"
COPY . .
WORKDIR "/src/ExternalDataImporter"
RUN dotnet build "ExternalDataImporter.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "ExternalDataImporter.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "ExternalDataImporter.dll"]
