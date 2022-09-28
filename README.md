# List Good command

[Swarm list info](#swarm-list-info)

[VOLUME SIZE](#volume-size)

[Show ip address container](#show-ip-address-container)


# Swarm list info
> docker node ls -q | xargs docker node inspect   -f '{{ .ID }} [hostname={{ .Description.Hostname }}, Addr={{ .Status.Addr }}, State={{ .Status.State }}, Role={{ .Spec.Role }}, Availability={{ .Spec.Availability }}]: Arch={{ .Description.Platform.Architecture }}, OS={{ .Description.Platform.OS }}, NanoCPUs={{ .Description.Resources.NanoCPUs }}, MemoryBytes={{ .Description.Resources.MemoryBytes }}, docker_version={{ .Description.Engine.EngineVersion }}, labels={{ range $k, $v := .Spec.Labels }}{{ $k }}={{ $v }} {{end}}'


# VOLUME SIZE

> ```for i in $(docker volume ls | awk '{print $2}' | tail -n +2); do sudo du -sh $(docker volume inspect -f "{{.Mountpoint}}" $i); done | sort -n -k 1```

# Show ip address container 

> docker inspect -f '{{range .NetworkSettings.Networks}} {{.IPAddress}}{{end}} %tab% {{.Name}}' $(docker ps -aq) | sed 's#%tab%#\t#g' | sed 's#/##g' | sort -t . -k 1,1n -k 2,2n -k 3,3n -k 4,4n

or 

> docker inspect -f '{{range .NetworkSettings.Networks}} {{.IPAddress}}{{end}} {{.Name}}' $(docker ps -aq) |  tr -d '/' |  sort -t . -k 4,4n
