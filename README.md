# Notes (Dendron)
This is a project that holds all my notes with markdown format.

I use Dendron (an note-taking extension in VSCode) to help organize/structure my notes.

Currently I am still getting use to this project.
## Best way to view the project
This project includes a VSCode workspace file.

The whole project can be easily opened with the `dendron.code-workspace` file in VSCode after cloning the project.

Then make sure `Dendron` extension is installed in VSCode to view the notes.

*vault* folder can be treated as a group of notes. It holds all the notes inside, and `Dendron` extension can help organize them with a tree view structure.

A project can have multiple vaults. But there is only 1 vault here that stores all my notes. (will re-organize it if there is a need to separate notes into different vaults.)
## Web page publishing upon commit
This project has GitHub Action configured to build the webpage as shown in https://ggluigi.github.io/mydocs/ every time new commit is made.

The formatted web page is built by linking the markdown notes I have created under `vault` folder.

~~And `docs` stores the files for webpage formats~~