# Tiltfile for resume-lambda-go

# checks
allow_k8s_contexts('docker-desktop')

# extensions
load('ext://dotenv', 'dotenv')

# environment variables
DOTENV = dotenv() or {}

# Define the Docker image for the Go Lambda service
docker_build(
  'resume-lambda-go-lambda',
  '.',
  dockerfile='Dockerfile',
  # live_update=[] # omit for golang it causes tar issues
)

# Define the interface service using nginx image
docker_build(
  'resume-lambda-go-interface',
  '.',
  dockerfile='interface/Dockerfile',
  live_update=[
    sync('interface/index.html', '/usr/share/nginx/html/index.html'),
    sync('interface/openapi.yaml', '/usr/share/nginx/html/openapi.yaml'),
    sync('interface/nginx.conf', '/etc/nginx/nginx.conf'),
  ]
)

# Use Helm to deploy the Lambda service
k8s_yaml(
  helm(
      'manifests',
      namespace='default',
      values=[
          'manifests/values.yaml'
      ],
      set=[
          'project_name=resume-lambda-go',
          'environment=development'
      ]
  )
)

# Forward the container port 8080 to the host port 18035
k8s_resource(
    workload='resume-lambda-go-lambda',
    port_forwards=[
        '18035:8080',  # http port
        # ":5678" # debug port (Go version doesn't use debugpy)
    ],
)

# Forward the container port 80 to the host port 18030
k8s_resource(
  workload='resume-lambda-go-interface',
  port_forwards=[
    '18030:80', # http port
  ],
  resource_deps=['resume-lambda-go-lambda']
)