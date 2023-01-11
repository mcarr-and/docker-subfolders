# copy in the .git folder allows you to do git commandsd
docker build  . -t muz/docker-git-acccess-root-folder;
docker run --name root-folder-git-success muz/docker-git-acccess-root-folder;

# no .git folder as you are off of the root.
cd subfolder-fail;
docker build . -t muz/docker-subfolder-git-fail;
docker run --name subfolder-git-fail muz/docker-subfolder-git-fail

# working file
cd ../subfolder-success
docker build --build-arg GIT_HASH=`git rev-parse --short HEAD` . -t muz/docker-subfolder-git-success;
docker run --name subfolder-git-success muz/docker-subfolder-git-success
