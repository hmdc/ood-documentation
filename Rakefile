
task :default do
  system "rake --tasks"
end

desc "Build docs using docker"
task :build do
  exec "#{run_cmd} make html"
end

desc "Spellcheck"
task :spellcheck do
  exec "#{run_cmd} make spellcheck"
end

desc "Open built documentation in browser"
task :open do
  exec '(command -v xdg-open >/dev/null 2>&1 && xdg-open build/html/index.html) || open build/html/index.html'
end

def user_group
  pwnam = Etc.getpwnam(Etc.getlogin)
  "#{pwnam.uid}:#{pwnam.gid}"
end

def image
  'ohiosupercomputer/ood-doc-build:v3.1.0'
end

def docker?
  `which docker 2>/dev/null 2>&1`
  $?.success?
end

def podman?
  `which podman 2>/dev/null 2>&1`
  $?.success?
end

def run_cmd
  if podman?
    "podman run --rm -it -v #{__dir__}:/doc #{image}"
  elsif docker?
    "docker run --rm -it -v '#{__dir__}:/doc' -u '#{user_group}' #{image}"
  else
    raise StandardError, "Cannot find any suitable container runtime to build. Need 'podman' or 'docker' installed."
  end
end
