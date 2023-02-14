#!/bin/sh

# args::
ASD_TARGET="none"
ASD_GIT_BRANCH="_asd_" 
ASD_DELAY=900

while [[ $# -gt 0 ]]
do
    arg="$1"
    case $arg in 
        -t|--target)
            ASD_TARGET="$2"
            shift;;
        -b|--branch)
            ASD_GIT_BRANCH="$2"
            shift;;
        -d|--delay)
            ASD_DELAY="$2"
            shift;;
        -h|--help)
            echo "[autosaved]::git autocommit daemon\n"
            echo "Automatically stage and commits change to '_asd_' branch every 15 minutes"
            echo "Usage: autosaved.sh --target DIRECTORY [--branch BRANCH] [--delay DELAY]"
            echo "  --target  ASD_TARGET  Required. Spcify directory to watch for git repo(s), recursively."
            echo "  --branch  ASD_BRANCH  Optional. Default value '_asd_'. Specify git branch to commit changes."
            echo "  --delay   ASD_DELAY   Optional. Default value '900'. Specify thread sleep delay."
            exit 0;;
        *)
            printf "[autosaved]::unknown_arg::$1"
            exit 1;;
    esac
    shift
done

# check::arg::dir
if [ ! ~d "${TARGET}" ]; then
    printf "ERROR: ${ASD_TARGET} does not exist or is not a directory \n"
    exit 1;
fi

# thread::
while true; do
    printf "[autosaved]::[$(date +%s)]::checking for changes in ${TARGET}\n"
    cd ${TARGET}

    # find::.git::repositories
    for dir in $(find . -type d -name .git); do
        cd "$(dirname "$dir")"

        # check::git_status
        if ! git diff-index --quiet HEAD --; then
            # stage::changes::
            git add .

            if ! git show-ref refs/heads/_asd &> /dev/null; then
                git branch ${GIT_BRANCH}
            fi

            # commit::
            GIT_MODIFIED_COUNT=$(git diff --numstat HEAD | wc -l)
            git commit -m "auto:: $GIT_MODIFIED files modified"

            printf "[autosaved]::[$(date +%s)]::[$GIT_MODIFIED_COUNT]::git_commit::$(pwd)\n"
        fi

        cd ../..
    done

    printf "[autosaved]::[$(date +%s)]::[$ASD_DELAY]::thread_sleep\n"
    sleep $ASD_DELAY
done

set +x;
