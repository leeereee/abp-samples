FROM mcr.microsoft.com/dotnet/core/aspnet:3.1-buster-slim AS base
WORKDIR /app
EXPOSE 80

FROM mcr.microsoft.com/dotnet/core/sdk:3.1-buster AS build
WORKDIR /src
COPY ["microservices/TenantManagementService.Host/TenantManagementService.Host.csproj", "microservices/TenantManagementService.Host/"]
COPY ["shared/MsDemo.Shared/MsDemo.Shared.csproj", "shared/MsDemo.Shared/"]
RUN dotnet restore "microservices/TenantManagementService.Host/TenantManagementService.Host.csproj" -nowarn:msb3202,msb3277,nu1503
COPY . .
WORKDIR "/src/microservices/TenantManagementService.Host"
RUN dotnet build "TenantManagementService.Host.csproj" -nowarn:msb3202,msb3277,nu1503 -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "TenantManagementService.Host.csproj"-nowarn:msb3202,msb3277,nu1503 -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "TenantManagementService.Host.dll"]