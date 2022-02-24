task :get_image_version do
  dockerfile_content = IO.read("transport-site/Dockerfile")
  version = dockerfile_content[/FROM (hexpm\/elixir.*)/, 1]
  version = version.gsub('hexpm/elixir:','elixir-')
  tools_version = dockerfile_content[/transport-tools:v(\d+\.\d+\.\d+) as transport-tools/, 1]
  fail "Unexpected FROM value (got #{version}), script must be adapted?" unless version =~ /\Aelixir\-[^\-]+\-erlang\-[^\-]+\-ubuntu\-focal\-[^\-]+\z/
  puts version + "-transport-tools-" + tools_version
end
