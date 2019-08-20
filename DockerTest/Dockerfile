FROM microsoft/dotnet:2.2-aspnetcore-runtime AS base
WORKDIR /app
EXPOSE 8812
EXPOSE 443

FROM microsoft/dotnet:2.2-sdk AS build
WORKDIR /src
COPY ["DockerTest/DockerTest.csproj", "DockerTest/"]
RUN dotnet restore "DockerTest/DockerTest.csproj"
COPY . .
WORKDIR "/src/DockerTest"
RUN dotnet build "DockerTest.csproj" -c Release -o /app

FROM build AS publish
RUN dotnet publish "DockerTest.csproj" -c Release -o /app

FROM base AS final
WORKDIR /app
COPY --from=publish /app .
#指定监听端口[, "--server.urls", "http://*:8810"],同时需要修改expose
ENTRYPOINT ["dotnet", "DockerTest.dll", "--server.urls", "http://*:8812"]