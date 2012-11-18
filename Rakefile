require 'fileutils'

desc 'Install hattori clock'
task :install => 'install:all'

AUDIO_FILE = '.cuckoo'
AUDIO_DIR = File.expand_path('~/')
APPLICATION_FILE = 'hattori_clock'
APPLICATION_DIR = '/usr/local/bin'
AGENT_FILE = 'org.polog.hattoriclock.plist'
AGENT_DIR = File.expand_path('~/Library/LaunchAgents/')

namespace :install do
  task :all => [ :audio, :application, :agent, :done ]

  desc 'Place audio file'
  task :audio do
    audio_source = 'data/cuckoo.m4a'
    audio_dest = File.join(AUDIO_DIR, AUDIO_FILE)

    cp audio_source, audio_dest
  end

  desc 'Place application file'
  task :application do
    application_source = File.join('bin', APPLICATION_FILE)
    application_dest = File.join(APPLICATION_DIR, APPLICATION_FILE)

    cp application_source, application_dest
    chmod 0755, application_dest
  end

  desc 'Place and load launch agent'
  task :agent do
    agent_source = File.join('data', AGENT_FILE)
    Dir.mkdir(AGENT_DIR) unless File.exists?(AGENT_DIR)
    agent_dest = File.join(AGENT_DIR, AGENT_FILE)

    cp agent_source, agent_dest
    sh "launchctl load -w #{agent_dest}"
  end

  task :done do
    if system(File.join(APPLICATION_DIR, APPLICATION_FILE))
      puts 'done'
    else
      puts 'failed to install'
    end
  end
end

desc 'Uninstall hattori clock'
task :uninstall => 'uninstall:all'

namespace :uninstall do
  task :all => [ :audio, :application, :agent, :done ]

  desc 'Remove audio file'
  task :audio do
    audio_dest = File.join(AUDIO_DIR, AUDIO_FILE)

    rm audio_dest
  end

  desc 'Remove application file'
  task :application do
    application_dest = File.join(APPLICATION_DIR, APPLICATION_FILE)

    rm application_dest
  end

  desc 'Remove and unload launch agent'
  task :agent do
    agent_dest = File.join(AGENT_DIR, AGENT_FILE)

    sh "launchctl unload -w #{agent_dest}"
    rm agent_dest
  end

  task :done do
    if system(File.join(APPLICATION_DIR, APPLICATION_FILE), '&> /dev/null')
      puts 'failed to uninstall'
    else
      puts 'removed'
    end
  end
end
