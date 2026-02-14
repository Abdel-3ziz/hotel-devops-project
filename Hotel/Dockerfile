FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src

COPY Hotel.csproj ./
RUN dotnet restore

COPY . .
RUN dotnet publish Hotel.csproj -c Release -o /app/out

FROM mcr.microsoft.com/dotnet/aspnet:6.0
WORKDIR /app

COPY --from=build /app/out .

EXPOSE 80
ENV ASPNETCORE_URLS=http://+:80
ENTRYPOINT ["dotnet", "Hotel.dll"]
