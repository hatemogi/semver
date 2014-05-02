SOURCE = 'semver.both.md'
EN = 'semver.en.md'
KO = 'semver.ko.md'

task default: :build
task build: [KO, EN, 'README.md']
file SOURCE

def inject_line_to_file filename, &block
  File.open(filename, "w") do |f|
    IO.readlines(SOURCE).each do |l|
      if (o = yield l)
        f.puts o
      end
    end
  end
end

desc '한국어 번역분 추출'
file KO => SOURCE do |t|
  inject_line_to_file t.name do |l|
    l.start_with?('>') ? nil : l
  end
end

desc '원본 추출'
file EN => SOURCE do |t|
  inject_line_to_file t.name do |l|
    if l.strip.empty?
      l
    else
      l.start_with?('>') ? l[2..-1] : false
    end
  end
end

desc 'intro.md와 ko를 합쳐서 README.md로'
file 'README.md' => [KO, 'intro.md'] do
  `cat intro.md #{KO} > README.md`
end


desc 'clean up previously built files'
task :clean do
  rm [KO, EN]
end

task :diff do
  sh "diff #{EN} semver.md"
end

task :all => [:clean, :build, :diff]
