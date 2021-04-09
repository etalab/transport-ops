task :get_image_version do
  version = IO.read("transport-site/Dockerfile")[/FROM (hexpm\/elixir.*)/, 1]
  version = version.gsub('hexpm/elixir:','elixir-')
  fail "Unexpected FROM format, script must be verified" unless version =~ /\Aelixir\-[^\-]+\-erlang\-[^\-]+\-alpine\-[^\-]+\z/
  puts version
end
