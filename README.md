### NewRelic
---
https://github.com/newrelic


---
#### centurion
https://github.com/newrelic/centurion

```
gem install centurion
centurionize -p <your_project>

bundle exec centurion -p radio-radio -e staging -a rolling_deploy
bundle exec centurion -p radio-radio -e staging -a deploy
bundle exec centurion -p radio-radio -e staging -a deploy_console
bundle exec centurion -p radio-radio -e staging -a repair
bundle exec centurion -p radio-radio -e staging -a list:running_container_tags
bundle exec centurion -p radio-radio -e staging -a list:running_containers
bundle exec centurion -p radio-radio -e staging -a list
centurion -e development -a deploy -p radio-radio --override-env=SERVICE_PORT=8000,NAME=radio
bundle exec centurion -e development -p testing_project -a dev:export_only
bundle exec rake spec

```

```ruby
namespace :environment
  task :common do
    set :image, 'example.com/newrelic/radio-radio'
    host 'docker-server-1.example.com'
    host 'docker-server-2.example.com'
  end
  desc 'Staging environment'
  task :staging => :common do
    set_current_environment(:staging)
    env_vars YOUR_ENV: 'staging'
    env_vars MY_DB: 'radio-db.example.com'
    host_port 10234, container_port: 9292
    host_port 10235, container_port: 9293
    host_volume '/mnt/volume1', container_volume: '/mnt/volume2'
  end
  desc 'Production environment'
  task :production => :common do
    set_current_environment(:production)
    env_vars YOUR_ENV: 'production'
    env_vars MY_DB: 'radio-db-prod.example.com'
    host_port 22234, container_port: 9292
    host_port 23235, container_port: 9293
    command ['/bin/bash', '-c', '/path/to/server -e production']
  end
end

desc 'Host-specific env vars'
task :production => :common do
  env_vars MEMBER_ID: lambda do |hostname|
    {
      'web-server1.company.net' => 'machine1'
      'web-server-2.company.net' => 'machine2'
    }[hostname]
  end
end

task :production => :common do
  set :dns, [ '172.17.42.1' ]
end

task :common do
  set :name, 'backend'
end

namespace :environment do
  task :common do
    set :image, 'example.com/newrelic/radio-radio'
    host 'docker-server-1.example.com'
    labels team: 'radio-ops'
  end
  desc 'Staging environment'
  task :staging => common do
    labels environment: 'radio-staging'
    env_vars YOUR_ENV: 'staging'
  end
end

set :container_hostname, 'yourhostname'

set :container_hostname, ->(hostname){ "#{hostname}.example.com" }

set :container_hostname, ->(hostname) { hostname }

set :network_mode, 'networkmode'

set :pid_mode, 'pidmode'

memory 1.gigabyte
cpu_shares 512

add_capability 'SOME_CAPABILITY'
add_capability 'ANOTHER_CAPABILITY'
drop_capability 'SOMEOTHER_CAPABILITY'

add_capability 'ALL'
drop_capability 'SOME_CAPABILITY'

add_security_opt 'seccomp=/path/to/seccomp/profile.json'
add_security_opt 'seccomp=unconfined'

task :production => :common do
  set :tlsverify, true
end

task :production => :common do
  set :tlsverify, true
  set :tlscacert, '/usr/local/certs/ca.pem'
  set :tlscert, '/usr/local/certs/ssl.crt'
  set :tlskey, '/usr/local/certs/ssl.key'
end

task :common do
  set :ssh, true
  set :ssh_user, "myuser"
  set :ssh_log_level, Logger::DEBUG
end


def cluster_green?(target_server, port, endpoint)
  response = begin
    Excon.get("http://#{target_server.hostname}:#{port}#{endpoint}")
  rescue Excon::Errors::SocketError
    warn "Elasticserch node not yet up"
    nil
  end
  return false unless response
  !JSON.parse(response)['timed_out']
end
task :producton => :common do
  set_current_environment(:production)
  set :status_endpoint, ""
  health_check method(:cluster_green?)
  host_port 9200, container_port: 9200
  host 'es-01.example.com'
  host 'es-02.example.com'
end


namespace :environment do
  task :common do
    registry :dogestry
    set :aws_access_key_id, 'abc123'
    set :aws_secret_key, 'xyz'
    set :s3_bucket, 'docker-images-bucket'
    set :s3_region, 'us-east-1'
  end
end

```

```
"Labels": {
  "team": "radio-ops",
  "environment": "radio-staging"
}
```

---
#### rpm
https://github.com/newrelic/rpm

```
gem 'newrelic_rpm'
newrelic install --license_key="YOUR_KEY" "My application"
gem 'newrelic_rpm'

```

```ruby
config.gem "newrelic_rpm"

require 'newrelic_rpm'
require 'newrelic_rpm'
NewRelic::Agent.manual_start


```

```

```



