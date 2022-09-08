# quantum-computing
Documentation repository for quantum-computing

## How to work with builds

This repo has a lot of automation set up. Follow these steps to update and build content:
1. Make all changes to the `source` branch.
2. Push commits to the `source` branch. No need to create pull requests.
3. Sit and wait for the preview (draft) version of the documentation to be built here: https://test.cloud.ibm.com/docs/quantum-computing
4. Every time a commit is made, a PR is automatically made, called "Next publish push". Merge this PR to push all changes to the production branch.
5. Sit and wait for the production version of the documentation to be built here: https://cloud.ibm.com/docs/quantum-computing

## How to add new files

In order to appear in the table of contents, add all new files to TOC.yaml, following the patterns that are already in place. Refer to the cloud TOC documentation if you want to do anything tricky: https://test.cloud.ibm.com/docs/writing?topic=writing-code-nav

## How to add tagged content

We can use tags to control what information shows up where. In general (and in these instructions), we use it to add draft content that we don't want to go live, even when other updates go live. 

1. The tag needs to be added to the feature-flags.json file. "Location" specifies where the output goes. In this example, the content will show up in the test environment but will NOT show up in the production environment. For example: 
```md
   {
        "name": "migration",
        "location": "draft"
    },
```    
2. Add this tag to any content that you add, in the TOC and around any new content. It can be around the whole file, just a few paragraphs, items in the Table of Contents, etc. For example:
```md
   
```
3. Check in the changes to the `source` branch as you normally would.
