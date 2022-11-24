#!/bin/sh
# simple trash cli for linux
# `trash help` for help message
# author: Roan Rothrock
# license: GNU General Public License

if [ -z "$1" ] || [ $1 = "--help" ]; then

  echo "Usage: trash [OPTIONS/FILES/DIRS]"
  echo "Options:"
  echo "  --delete/--remove [FILES/DIRS]	deletes given files from trash (interactive)"
  echo "  --empty                       empties trash (immediate)"
  echo "  --show                        shows files in trash"
  echo "  --help                        shows this message and exits"
  echo "  --restore [FILES/DIRS]        prompts where you want each file to be moved to,"
  echo "                                then moves them there"
  echo "  [FILES/DIRS]                  adds given files or directories to trash"

  exit 0
fi

trash_dir=~/.local/share/Trash/files
starting_dir="$(pwd)"

mkdir -p $trash_dir # creating trash if it doesn't exist

if [ "$1" = "--delete" ] || [ "$1" = "--remove" ]; then # empties trash can file one by one
  cd $trash_dir

  if [ -z "$2" ]; then
    echo "Trash: Error: \`delete\` is supposed to be used with arguments. \`trash help\` for help."
  else
    for arg in "${@:2}"; do
      if [ -e "$arg" ] || [ -d "$arg" ]; then
        read -p "Trash: Do you want to delete \`$arg\` [y\n]: " res
        if [ "$res" = "y" ]; then
          rm -r -f "$arg"
        fi
      else
        echo "Trash: Error: File or directory not found: $arg"
      fi
    done
  fi

  cd "$starting_dir"

elif [ "$1" = "--empty" ]; then # empties trash can (immediate)

  cd $trash_dir
  rm -rf ..?* .[!.]* *
  cd "$starting_dir"

elif [ "$1" = "--show" ]; then # shows items in trash can

  if [ -z "$(ls -A $trash_dir)" ]; then # trash is empty
    echo "Trash: Your trash is currently empty."
  else
    ls -A $trash_dir --color=auto
  fi

elif [ "$1" = "--restore" ]; then # restores items from trash

  cd $trash_dir

  if [ -z "$2" ]; then
    echo "Trash: Error: `delete` is supposed to be used with arguments. `trash help` for help."
  else
    for arg in "${@:2}"; do
      if [ -e "$arg" ] || [ -d "$arg" ]; then # reads input for where to put items
        read -p "Trash: Where would you like to restore \`$arg\`: ~/" location
        mv "$arg" "~/$location/$arg"
      else
        echo "Trash: Error: File or directory not found: $arg"
      fi
    done
  fi

  cd "$starting_dir"

else # adds files to trash can

  for arg in "$@"; do
    if [ -e "$arg" ] || [ -d "$arg" ]; then # adds items to trash can
      mv "$arg" "$trash_dir"
      echo "Trash: Added to trash: $arg"
    else
      echo "Trash: Error: file or directory not found: $arg"
    fi
  done
fi

exit 0


