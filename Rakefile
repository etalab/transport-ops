task :get_image_version do
  version = IO.read("transport-site/Dockerfile")[/FROM (hexpm\/elixir.*)/, 1]
  version = version.gsub('hexpm/elixir:','elixir-')
  fail "Unexpected FROM value (got #{version}), script must be adapted?" unless version =~ /\Aelixir\-[^\-]+\-erlang\-[^\-]+\-ubuntu\-focal\-[^\-]+\z/
  puts version
end
