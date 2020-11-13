require "rake"

QMK_DIR = "qmk"
KEYMAP_NAME = "jimmy"
KEYBOARDS = [
  ['crkbd', 'rev1', 'avrdude']
]

task default: :install

task :install do 
  sh "brew bundle"

  unless File.exist?(QMK_DIR)
    sh "git clone git@github.com:qmk/qmk_firmware.git #{QMK_DIR}"
  end

 Dir.chdir(QMK_DIR) do
    sh "git pull"
    sh "make git-submodule"
    sh "qmk setup ."
  end

  KEYBOARDS.each do |(keyboard, rev, bootloader)|
    source_dir = File.expand_path(keyboard)
    link_dir = "#{QMK_DIR}/keyboards/#{keyboard}/keymaps/#{KEYMAP_NAME}"

    if File.symlink?(link_dir)
      FileUtils.rm_r(link_dir)
    end

    FileUtils.ln_s(source_dir, link_dir)
  end
end

KEYBOARDS.each do |(keyboard, rev, bootloader)|
  namespace keyboard do
    task :flash do
      Dir.chdir(QMK_DIR) do
        sh "make #{keyboard}/#{rev}:#{KEYMAP_NAME}:#{bootloader}"
      end
    end
  end
end
