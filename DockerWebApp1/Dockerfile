FROM mcr.microsoft.com/dotnet/core/aspnet:2.1-stretch-slim AS base
WORKDIR /app


FROM mcr.microsoft.com/dotnet/core/sdk:2.1-stretch AS build
WORKDIR /src
#COPY ["DockerWebApp1/DockerWebApp1.csproj", "DockerWebApp1/"]
#This change is required to provide correct build context path. When debugging use the above COPY command
COPY . DockerWebApp1/
RUN dotnet restore "DockerWebApp1/DockerWebApp1.csproj"
COPY . .
WORKDIR "/src/DockerWebApp1"
RUN dotnet build "DockerWebApp1.csproj" -c Release -o /app

FROM build AS publish
RUN dotnet publish "DockerWebApp1.csproj" -c Release -o /app

FROM base AS final
WORKDIR /app
COPY --from=publish /app .
ENV ASPNETCORE_URLS http://*:8080
ENTRYPOINT ["dotnet", "DockerWebApp1.dll"]