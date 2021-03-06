cmake_minimum_required(VERSION 3.16.3)

project(OAuth2CLI VERSION 0.1.1)

find_package(Python3 REQUIRED)

# Create the python3 virtual environment.
add_custom_command(
  OUTPUT venv
  COMMAND python3
  ARGS -m venv venv)

# Install oauth2_cli dependencies into python3 virtual environment.
add_custom_command(
  OUTPUT venv.stamp requirements.txt venv_install.log
  DEPENDS venv ${CMAKE_CURRENT_SOURCE_DIR}/requirements.txt
  COMMAND ${CMAKE_COMMAND} -E copy ${CMAKE_CURRENT_SOURCE_DIR}/requirements.txt requirements.txt
  COMMAND touch venv.stamp
  COMMAND ./venv/bin/pip install -r requirements.txt >| venv_install.log )

# Build the oauth2_cli standalone executable.
add_custom_command(
  OUTPUT dist build oauth2_cli.py oauth2_cli.spec __pycache__ oauth2_cli
  DEPENDS venv.stamp ${CMAKE_CURRENT_SOURCE_DIR}/oauth2_cli.py
  COMMAND ${CMAKE_COMMAND} -E copy ${CMAKE_CURRENT_SOURCE_DIR}/oauth2_cli.py oauth2_cli.py
  COMMAND ./venv/bin/pyinstaller --onefile --log-level WARN oauth2_cli.py
  COMMAND ${CMAKE_COMMAND} -E copy ${CMAKE_CURRENT_BINARY_DIR}/dist/oauth2_cli oauth2_cli)

add_custom_target(oauth2_cli_target
  ALL
  DEPENDS oauth2_cli)

install(
  PROGRAMS ${CMAKE_CURRENT_BINARY_DIR}/oauth2_cli
  DESTINATION bin)
