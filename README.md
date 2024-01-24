#! /usr/bin/env bash
#main Function
main() {
    while true; do
        echo "Please select one task from the following options:"
        echo "1) Backup File"
        echo "2) Rename File"
        echo "3) Delete File"
        echo "4) Check Disk Space"
        echo "5) Exit"

        read -p "Enter Your choice (1-5):" choice # getting input from user
# depending on user's (choice) specific function will be executed
        case $choice in 
            1) backup ;;
            2) rename ;;
            3) delete ;;
            4) checkFileInfo ;;
            5) exit ;;
            *) echo "Invalid option";;
        esac
    done
}
# It tasks Functions:
backup() { # creating a backup function

echo  "Enter the name of a file:"
read  file

if [ -f "$file" ]; then # check if file exist
  backup="${file}.bak" # backup file name with a .bak extension
  if [ -f  "$backup" ]; then  #  checking if file backup with .bak extension exists
  # If it does exist then user will be asked if he wants to overwrite the existing backup file
          echo "Backup file already exists! Do you want to replace it?(y/n)"
          read  answer
          if [ "$answer" = "y" ]; then
                  filename="${file%.*}" #extracting the file name without the extension after '.' and storing it in the variable filename
                  extension="${file##*.}" #extracting the extension after '.' and storing it in the variable extension
                  newBackup="${filename}_newBackup.${extension}.bak"
                  cp "$file" "$newBackup" #creating a new backup file name with the extension .bak
                  echo "New Backup created $newBackup"
                  ls #display files in a directory
          else 
                  echo "You chose not to backup"
          fi
  else 
          cp "$file" "$backup" #copy file to backup file
          echo "Backup created $backup"
          ls # # display files in a directory
  fi
else 
echo "File does not exist"
fi
}
rename () { # creating a rename function

  echo "Enter File  You Want to Rename:"
  read fileName # reading file name from user
  
  if [ -f "$fileName" ]; then   # checking if file exists
    echo "Enter New Name:"
    read newName #reading new file name from the user
    mv "$fileName" "$newName" # renaming the file
    echo "File $fileName was renamed to  $newName"
    ls # # display files in a directory
  else 
    echo "File $fileName does not exist"
  fi
}
delete () { # creating a delete function

echo " How many Files do You want to delete"
read num # reading number of files the user wants to delete 

for  (( i=1; i<=$num; i++ )); do # for loop to delete more than one file 
  echo " Enter File Name $i:" 
  read fileName # reading file name from user
  if [ -f "$fileName" ]; then   # checking if file exists
    rm -i "$fileName" # deleting the file
    echo "File $fileName was deleted"
    ls
  else
    echo "File $fileName does not exist"
  fi
done
}
  
checkFileInfo (){

  echo "Checking Current Directory Files Info"
  echo "Information Obtained"
  fileInfo=$(ls -lh) 
  # Display files in directory with human-readable size and other info
  # store it in fileInfo variable
  echo "File Permissions| # of Links | Owner | Group | Size |Last Modified| File Name"
  echo "$fileInfo"
}

main
    

