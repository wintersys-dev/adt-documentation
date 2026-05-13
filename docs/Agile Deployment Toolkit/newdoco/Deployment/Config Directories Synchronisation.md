To enable the tool based config directory synchronisation process (recommended), set the following

${BUILD_HOME}/configuration/software.dat

DATASTORECONFIGSTYLE:tool
#DATASTORECONFIGSTYLE:lightweight
#DATASTORECONFIGSTYLE:heavyweight

To enable the heavyweight config synchronisation process, set the following

${BUILD_HOME}/configuration/software.dat

#DATASTORECONFIGSTYLE:tool
#DATASTORECONFIGSTYLE:lightweight
DATASTORECONFIGSTYLE:heavyweight


To enable the lightweight config directory synchronisation process, set the following

${BUILD_HOME}/configuration/software.dat

#DATASTORECONFIGSTYLE:tool
DATASTORECONFIGSTYLE:lightweight
#DATASTORECONFIGSTYLE:heavyweight
