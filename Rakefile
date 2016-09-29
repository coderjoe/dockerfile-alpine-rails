# Build rake tasks for building, running, and cleaning
# images in subdirectories with Dockerfiles

all_images = Dir.glob('./*/Dockerfile').map do |d| 
  File.basename(File.dirname(d))
end

all_images.each do |image_name|
  namespace "#{image_name}" do
    desc "Build the #{image_name} docker image"
    task :build do
      system("docker build -t \"#{image_name}\" #{image_name}")
    end

    desc "Run 'docker run <options> #{image_name} <command>' " \
         "(default options: -it --rm, command: /bin/sh)"
    task :run, [:command, :options] => [:build] do |t, args|
      args.with_defaults(
        command: '/bin/sh',
        options: '-it --rm'
      )
      system("docker run #{args[:options]} \"#{image_name}\" #{args[:command]}")
    end

    desc "Delete the #{image_name} image"
    task :delete do
      system("docker rmi \"#{image_name}\"")
    end
  end

  desc "Run #{image_name}:run"
  task "#{image_name}" => ["#{image_name}:run"]
end

desc "Build all docker images"
task :build => all_images.map { |i| "#{i}:build" }

desc "Delete all docker images"
task :delete => all_images.map { |i| "#{i}:delete" }

task :default => [:build]
