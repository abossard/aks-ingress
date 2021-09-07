FROM mcr.microsoft.com/dotnet/aspnet:5.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:5.0 AS build
WORKDIR /src
COPY ["webapp1.csproj", "."]
RUN dotnet restore "webapp1.csproj"
COPY . .
RUN dotnet build "webapp1.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "webapp1.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "webapp1.dll"]
