FROM microsoft/dotnet:3.0-aspnetcore-runtime-stretch-slim AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM microsoft/dotnet:3.0-sdk-stretch AS build
WORKDIR /src
COPY ["dockertest/dockertest.csproj", "dockertest/"]
RUN dotnet restore "dockertest/dockertest.csproj"
COPY . .
WORKDIR "/src/dockertest"
RUN dotnet build "dockertest.csproj" -c Release -o /app

FROM build AS publish
RUN dotnet publish "dockertest.csproj" -c Release -o /app

FROM base AS final
WORKDIR /app
COPY --from=publish /app .
ENTRYPOINT ["dotnet", "dockertest.dll"]
