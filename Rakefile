require 'bundler/setup'
require 'bundler/gem_tasks'
require 'tentd/tasks/db'

namespace :status do
  task :configure do
    require 'tent-status/compiler'

    public_dir = File.expand_path(File.join(File.dirname(__FILE__), 'public'))
    TentStatus::Compiler.assets_dir = File.join(public_dir, 'assets')
    TentStatus::Compiler.layout_dir = public_dir
    TentStatus::Compiler.layout_path = File.join(TentStatus::Compiler.layout_dir, 'status.html')

    TentStatus::Compiler.configure_app(
      :url => "#{ENV['URL']}/status",
      :path_prefix => '/status',
      :asset_root => '/assets',
      :json_config_url => "#{ENV['URL']}/status/config.json",
      :admin_url => "/admin",
      :signout_url => "/signout"
    )
  end

  require 'tent-status/tasks/assets'
  require 'tent-status/tasks/layout'
end

namespace :admin do
  task :configure do
    require 'tent-admin/compiler'
    require 'tent-status'

    public_dir = File.expand_path(File.join(File.dirname(__FILE__), 'public'))
    TentAdmin::Compiler.assets_dir = File.join(public_dir, 'assets')
    TentAdmin::Compiler.layout_dir = public_dir
    TentAdmin::Compiler.layout_path = File.join(TentAdmin::Compiler.layout_dir, 'admin.html')

    TentAdmin::Compiler.configure_app(
      :url => "#{ENV['URL']}/admin",
      :path_prefix => '/admin',
      :asset_root => '/assets',
      :json_config_url => "#{ENV['URL']}/admin/config.json",
      :status_url => "/status",
      :search_url => TentStatus.settings[:search_enabled] ? "/status/search" : nil,
      :signout_url => "/signout"
    )
  end

  require 'tent-admin/tasks/assets'
  require 'tent-admin/tasks/layout'
end

namespace :assets do
  desc "Precompile Status and Admin apps"
  task :precompile => [
    "status:configure",
    "status:assets:precompile",
    "status:layout:compile",
    "admin:configure",
    "admin:assets:precompile",
    "admin:layout:compile"
  ] do
  end
end

task :encrypt_passphrase, :passphrase do |t, args|
  require 'bcrypt'
  puts BCrypt::Password.create(args[:passphrase])
end
