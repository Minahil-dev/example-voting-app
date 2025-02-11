# This stage builds the .NET application.
FROM mcr.microsoft.com/dotnet/sdk:7.0 AS build

# Set the target architecture to amd64 (Intel/AMD platforms)
ARG TARGETPLATFORM
ARG TARGETARCH
ARG BUILDPLATFORM
RUN echo "I am running on $BUILDPLATFORM, building for $TARGETPLATFORM"

WORKDIR /source

# Copy the .csproj file and restore any dependencies (via dotnet restore)
COPY *.csproj ./
RUN dotnet restore -a $TARGETARCH

# Copy the rest of the application and publish it
COPY . ./
RUN dotnet publish -c release -o /app -a $TARGETARCH --self-contained false --no-restore

# This stage sets up the runtime environment for the application.
FROM mcr.microsoft.com/dotnet/runtime:7.0

WORKDIR /app

# Copy the compiled application from the build stage
COPY --from=build /app ./

# Set the entry point for the application
ENTRYPOINT ["dotnet", "Worker.dll"]
