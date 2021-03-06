#######################################
# build script (Ruby make)
#
#  use:
#   $ rake   or
#   $ rake build     - to build football.db from scratch
#
#   $ rake update    - to update football.db
#
#   $ rake book    - build book
#
#   $ rake -T        - show all tasks


BUILD_DIR = "./build"

# -- output db config
FOOTBALL_DB_PATH = "#{BUILD_DIR}/football.db"


# -- input repo sources config
OPENMUNDI_ROOT = "../../openmundi"
OPENFOOTBALL_ROOT = ".."

WORLD_DB_INCLUDE_PATH = "#{OPENMUNDI_ROOT}/world.db"

STADIUMS_INCLUDE_PATH  = "#{OPENFOOTBALL_ROOT}/stadiums"

WORLD_CUP_INCLUDE_PATH              = "#{OPENFOOTBALL_ROOT}/world-cup"
EURO_CUP_INCLUDE_PATH               = "#{OPENFOOTBALL_ROOT}/euro-cup"
AFRICA_CUP_INCLUDE_PATH             = "#{OPENFOOTBALL_ROOT}/africa-cup"
NORTH_AMERICA_GOLD_CUP_INCLUDE_PATH = "#{OPENFOOTBALL_ROOT}/north-america-gold-cup"
COPA_AMERICA_INCLUDE_PATH           = "#{OPENFOOTBALL_ROOT}/copa-america"


WORLD_INCLUDE_PATH       = "#{OPENFOOTBALL_ROOT}/world"

EUROPE_INCLUDE_PATH      = "#{OPENFOOTBALL_ROOT}/europe"
AT_INCLUDE_PATH          = "#{OPENFOOTBALL_ROOT}/at-austria"
DE_INCLUDE_PATH          = "#{OPENFOOTBALL_ROOT}/de-deutschland"
EN_INCLUDE_PATH          = "#{OPENFOOTBALL_ROOT}/en-england"
ES_INCLUDE_PATH          = "#{OPENFOOTBALL_ROOT}/es-espana"
IT_INCLUDE_PATH          = "#{OPENFOOTBALL_ROOT}/it-italy"

EUROPE_CHAMPIONS_LEAGUE_INCLUDE_PATH = "#{OPENFOOTBALL_ROOT}/europe-champions-league"


AMERICA_INCLUDE_PATH     = "#{OPENFOOTBALL_ROOT}/america"
MX_INCLUDE_PATH          = "#{OPENFOOTBALL_ROOT}/mx-mexico"
BR_INCLUDE_PATH          = "#{OPENFOOTBALL_ROOT}/br-brazil"

NORTH_AMERICA_CHAMPIONS_LEAGUE_INCLUDE_PATH = "#{OPENFOOTBALL_ROOT}/north-america-champions-league"
COPA_LIBERTADORES_INCLUDE_PATH              = "#{OPENFOOTBALL_ROOT}/copa-libertadores"
COPA_SUDAMERICANA_INCLUDE_PATH              = "#{OPENFOOTBALL_ROOT}/copa-sudamericana"


DB_CONFIG = {
  adapter:    'sqlite3',
  database:   FOOTBALL_DB_PATH
}


#######################
#  print settings

settings = <<EOS
*****************
settings:
  WORLD_DB_INCLUDE_PATH: #{WORLD_DB_INCLUDE_PATH}

  STADIUMS_INCLUDE_PATH: #{STADIUMS_INCLUDE_PATH}

  WORLD_CUP_INCLUDE_PATH:               #{WORLD_CUP_INCLUDE_PATH}
  EURO_CUP_INCLUDE_PATH:                #{EURO_CUP_INCLUDE_PATH}
  AFRICA_CUP_INCLUDE_PATH:              #{AFRICA_CUP_INCLUDE_PATH}
  NORTH_AMERICA_GOLD_CUP_INCLUDE_PATH:  #{NORTH_AMERICA_GOLD_CUP_INCLUDE_PATH}
  COPA_AMERICA_INCLUDE_PATH:            #{COPA_AMERICA_INCLUDE_PATH}

  WORLD_INCLUDE_PATH:        #{WORLD_INCLUDE_PATH}
  AMERICA_INCLUDE_PATH:      #{AMERICA_INCLUDE_PATH}
  DE_INCLUDE_PATH:           #{DE_INCLUDE_PATH}
  EN_INCLUDE_PATH:           #{EN_INCLUDE_PATH}
  ES_INCLUDE_PATH:           #{ES_INCLUDE_PATH}
  MX_INCLUDE_PATH:           #{MX_INCLUDE_PATH}
  BR_INCLUDE_PATH:           #{BR_INCLUDE_PATH}

  NORTH_AMERICA_CHAMPIONS_LEAGUE_INCLUDE_PATH: #{NORTH_AMERICA_CHAMPIONS_LEAGUE_INCLUDE_PATH}
  COPA_LIBERTADORES_INCLUDE_PATH:              #{COPA_LIBERTADORES_INCLUDE_PATH}
  COPA_SUDAMERICANA_INCLUDE_PATH:              #{COPA_SUDAMERICANA_INCLUDE_PATH}
*****************
EOS

puts settings




task :default => :build

directory BUILD_DIR


task :clean do
  rm FOOTBALL_DB_PATH if File.exists?( FOOTBALL_DB_PATH )
end


task :env => BUILD_DIR do
  require 'worlddb'   ### NB: for local testing use rake -I ./lib dev:test e.g. do NOT forget to add -I ./lib
  require 'sportdb'
  require 'logutils/db'

  LogUtils::Logger.root.level = :info

  pp DB_CONFIG
  ActiveRecord::Base.establish_connection( DB_CONFIG )
end


task :create => :env do
  LogDb.create
  WorldDb.create
  SportDb.create
end

task :importworld => :env do
  # populate world tables
  WorldDb.read_setup( 'setups/sport.db.admin', WORLD_DB_INCLUDE_PATH, skip_tags: true )
end

task :importbuiltin => :env do
  SportDb.read_builtin
  LogUtils::Logger.root.level = :debug
end

######################
# grounds / stadiums

task :grounds  => :importbuiltin do
  SportDb.read_setup( 'setups/all',   STADIUMS_INCLUDE_PATH )
end

#####################
# national teams

task :worldcup => :importbuiltin do
  SportDb.read_setup( 'setups/teams', EURO_CUP_INCLUDE_PATH )
  SportDb.read_setup( 'setups/teams', AFRICA_CUP_INCLUDE_PATH )
  SportDb.read_setup( 'setups/teams', NORTH_AMERICA_GOLD_CUP_INCLUDE_PATH )
  SportDb.read_setup( 'setups/teams', COPA_AMERICA_INCLUDE_PATH )
  SportDb.read_setup( 'setups/all',   WORLD_CUP_INCLUDE_PATH )
end

task :copaamerica => :importbuiltin do
  SportDb.read_setup( 'setups/teams', WORLD_CUP_INCLUDE_PATH )   # include for invitees e.g. JPN
  SportDb.read_setup( 'setups/teams', NORTH_AMERICA_GOLD_CUP_INCLUDE_PATH )   # include for invitees e.g. MEX, CRC
  SportDb.read_setup( 'setups/all',   COPA_AMERICA_INCLUDE_PATH )
end

task :goldcup  => :importbuiltin do
  SportDb.read_setup( 'setups/all', NORTH_AMERICA_GOLD_CUP_INCLUDE_PATH )
end

task :eurocup  => :importbuiltin do
  SportDb.read_setup( 'setups/all', EURO_CUP_INCLUDE_PATH )
end


################################
# football clubs n leagues

task :es => :importbuiltin do
  SportDb.read_setup( 'setups/all',   ES_INCLUDE_PATH )
end

task :world => :importbuiltin do
  SportDb.read_setup( 'setups/teams', EN_INCLUDE_PATH )
  SportDb.read_setup( 'setups/teams', DE_INCLUDE_PATH )
  SportDb.read_setup( 'setups/teams', MX_INCLUDE_PATH )
  SportDb.read_setup( 'setups/teams', BR_INCLUDE_PATH )
  SportDb.read_setup( 'setups/all',   WORLD_INCLUDE_PATH )
end

task :america => :importbuiltin do
  SportDb.read_setup( 'setups/all',   AMERICA_INCLUDE_PATH )
end

task :northamericachampionsleague => :importbuiltin do
  SportDb.read_setup( 'setups/teams', MX_INCLUDE_PATH )
  SportDb.read_setup( 'setups/teams', AMERICA_INCLUDE_PATH )
  SportDb.read_setup( 'setups/all',   NORTH_AMERICA_CHAMPIONS_LEAGUE_INCLUDE_PATH )
end

task :copalibertadores => :importbuiltin do
  SportDb.read_setup( 'setups/teams', MX_INCLUDE_PATH )  # include invitees (mx teams)
  SportDb.read_setup( 'setups/teams', BR_INCLUDE_PATH )
  SportDb.read_setup( 'setups/teams', AMERICA_INCLUDE_PATH )
  SportDb.read_setup( 'setups/all',   COPA_LIBERTADORES_INCLUDE_PATH )
end

task :copasudamericana => :importbuiltin do
  SportDb.read_setup( 'setups/teams', MX_INCLUDE_PATH )  # include invitees (mx teams) ???
  SportDb.read_setup( 'setups/teams', BR_INCLUDE_PATH )
  SportDb.read_setup( 'setups/teams', AMERICA_INCLUDE_PATH )
  SportDb.read_setup( 'setups/all',   COPA_SUDAMERICANA_INCLUDE_PATH )
end


#############
# Misc

task :all => [:importbuiltin, :grounds] do
  ########
  # national teams

  SportDb.read_setup( 'setups/teams',  EURO_CUP_INCLUDE_PATH )
  SportDb.read_setup( 'setups/teams',  AFRICA_CUP_INCLUDE_PATH )
  SportDb.read_setup( 'setups/teams',  NORTH_AMERICA_GOLD_CUP_INCLUDE_PATH )
  SportDb.read_setup( 'setups/teams',  COPA_AMERICA_INCLUDE_PATH )
  SportDb.read_setup( 'setups/teams',  WORLD_CUP_INCLUDE_PATH )

  ### fix!! -add setups/events  to setups; add all teams to setups/teams !!!!
  SportDb.read_setup( 'setups/all',   EURO_CUP_INCLUDE_PATH )
  SportDb.read_setup( 'setups/all',   AFRICA_CUP_INCLUDE_PATH )
  SportDb.read_setup( 'setups/all',   NORTH_AMERICA_GOLD_CUP_INCLUDE_PATH )
  SportDb.read_setup( 'setups/all',   COPA_AMERICA_INCLUDE_PATH )  # includes invitee from north_america (e.g. mexico,costa rica) & asia (e.g. japan)
  SportDb.read_setup( 'setups/all',   WORLD_CUP_INCLUDE_PATH )

  ################
  # clubs

  SportDb.read_setup( 'setups/teams',  WORLD_INCLUDE_PATH )
  SportDb.read_setup( 'setups/teams',  EUROPE_INCLUDE_PATH )
  SportDb.read_setup( 'setups/teams',  AT_INCLUDE_PATH )
  SportDb.read_setup( 'setups/teams',  DE_INCLUDE_PATH )
  SportDb.read_setup( 'setups/teams',  EN_INCLUDE_PATH )
  SportDb.read_setup( 'setups/teams',  ES_INCLUDE_PATH )
  SportDb.read_setup( 'setups/teams',  IT_INCLUDE_PATH )

  SportDb.read_setup( 'setups/teams',  AMERICA_INCLUDE_PATH )
  SportDb.read_setup( 'setups/teams',  MX_INCLUDE_PATH )  # include invitees (mx teams)
  SportDb.read_setup( 'setups/teams',  BR_INCLUDE_PATH )

  ### fix!! -add setups/events  to setups; add all teams to setups/teams !!!!
  # [debug] parsing game (fixture) line: >Sáb 5 Ene 19:30  Morelia      3-3  Cruz Azul<
  # [debug]      match for team  >cruzazul< >Cruz Azul<
  # [debug]      match for team  >morelia< >Morelia<
  # [debug]    team1: >morelia<
  # [debug]    team2: >cruzazul<
  # [debug]    score: >19-30<
  #[debug]   line: >Sáb 5 Ene [SCORE]  [TEAM1]       3-3  [TEAM2] <

  SportDb.read_setup( 'setups/all',  AT_INCLUDE_PATH )
  SportDb.read_setup( 'setups/all',  DE_INCLUDE_PATH )
  SportDb.read_setup( 'setups/all',  EN_INCLUDE_PATH )
  SportDb.read_setup( 'setups/all',  ES_INCLUDE_PATH )
  SportDb.read_setup( 'setups/all',  IT_INCLUDE_PATH )
  SportDb.read_setup( 'setups/all',  EUROPE_CHAMPIONS_LEAGUE_INCLUDE_PATH )

  ## fix!! --
  ## SportDb.read_setup( 'setups/all', MX_INCLUDE_PATH )
  SportDb.read_setup( 'setups/all', NORTH_AMERICA_CHAMPIONS_LEAGUE_INCLUDE_PATH )
  SportDb.read_setup( 'setups/all',    BR_INCLUDE_PATH )
  SportDb.read_setup( 'setups/all',    COPA_LIBERTADORES_INCLUDE_PATH )

  SportDb.read_setup( 'setups/all',   WORLD_INCLUDE_PATH )  # circular reference; requires other teams
end


# - test sport.db.admin setup
task :admin => [:importbuiltin, :grounds] do
  ########
  # national teams

  SportDb.read_setup( 'setups/2012',   EURO_CUP_INCLUDE_PATH )
  SportDb.read_setup( 'setups/teams',  AFRICA_CUP_INCLUDE_PATH )
  SportDb.read_setup( 'setups/teams',  NORTH_AMERICA_GOLD_CUP_INCLUDE_PATH )
  SportDb.read_setup( 'setups/teams',  COPA_AMERICA_INCLUDE_PATH )
  SportDb.read_setup( 'setups/2014',   WORLD_CUP_INCLUDE_PATH )

  ################
  # clubs

  SportDb.read_setup( 'setups/teams',  WORLD_INCLUDE_PATH )
  SportDb.read_setup( 'setups/teams',  EUROPE_INCLUDE_PATH )

  SportDb.read_setup( 'setups/2013_14',  AT_INCLUDE_PATH )
  SportDb.read_setup( 'setups/2013_14',  DE_INCLUDE_PATH )
  SportDb.read_setup( 'setups/2013_14',  EN_INCLUDE_PATH )
  SportDb.read_setup( 'setups/2013_14',  ES_INCLUDE_PATH )
  SportDb.read_setup( 'setups/2013_14',  IT_INCLUDE_PATH )
  SportDb.read_setup( 'setups/2013_14',  EUROPE_CHAMPIONS_LEAGUE_INCLUDE_PATH )

  SportDb.read_setup( 'setups/teams',   AMERICA_INCLUDE_PATH )
  SportDb.read_setup( 'setups/2013_14', MX_INCLUDE_PATH )
  SportDb.read_setup( 'setups/2013_14', NORTH_AMERICA_CHAMPIONS_LEAGUE_INCLUDE_PATH )
  SportDb.read_setup( 'setups/2013',    BR_INCLUDE_PATH )
  SportDb.read_setup( 'setups/2013',    COPA_LIBERTADORES_INCLUDE_PATH )

  SportDb.read_setup( 'setups/2013',   WORLD_INCLUDE_PATH )  # circular reference; requires other teams
end


#########################################################
# note: change deps to what you want to import for now


task :importsport => [:all] do
  # nothing here
end


task :deletesport => :env do
  SportDb.delete!
end


desc 'build football.db from scratch (default)'
task :build => [:clean, :create, :importworld, :importsport] do
  puts 'Done.'
end

desc 'update football.db'
task :update => [:deletesport, :importsport] do
  puts 'Done.'
end

desc 'pull (auto-update) football.db from upstream sources'
task :pull => :env do
  SportDb.update!
  puts 'Done.'
end


desc 'build book (draft version) - The Free Football World Almanac - from football.db'
task :book => :env do

  PAGES_DIR = "#{BUILD_DIR}/pages"  # use PAGES_OUTPUT_DIR or PAGES_ROOT ??

  require './scripts/book'


  build_book()                # multi-page version
  build_book( inline: true )  # all-in-one-page version a.k.a. inline version

  puts 'Done.'
end


desc 'build book (release version) - The Free Football World Almanac - from football.db'
task :publish => :env do

  PAGES_DIR = "../book/_pages"  # use PAGES_OUTPUT_DIR or PAGES_ROOT ??

  require './scripts/book'

  build_book()                # multi-page version
  build_book( inline: true )  # all-in-one-page version a.k.a. inline version

  puts 'Done.'
end


desc 'print versions of gems'
task :about => :env do
  puts ''
  puts 'gem versions'
  puts '============'
  puts "textutils #{TextUtils::VERSION}     (#{TextUtils.root})"
  puts "worlddb   #{WorldDb::VERSION}     (#{WorldDb.root})"
  puts "sportdb   #{SportDb::VERSION}     (#{SportDb.root})"

  ## todo - add LogUtils  LogDb ??  - check for .root too
end


desc 'print stats for football.db tables/records'
task :stats => :env do
  puts ''
  puts 'world.db'
  puts '============'
  WorldDb.tables

  puts ''
  puts 'football.db'
  puts '==========='
  SportDb.tables
end



