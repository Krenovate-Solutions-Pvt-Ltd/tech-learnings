## **Setup Github action for your theme in order to deploy the new version of your theme to any host with SSH access.**

### Steps Involved

1. Setting up Github Repository
2. Setting up your host machine
3. Command to push code to Host from local

## **1. Setting up Github Repository**

We will start by adding actions.

1. Click on the "Actions" tab in your repository.
2. Click on the button "New Workflow" next. You will find it in the top left section. After this, Click on the button “Skip this and set up a workflow yourself → ”
3. You will be taken to a page where which would be at `your-repo/.github/workflows/main.yml`. Change the name of the file to `live.yml` as we are creating actions to deploy our code to the live server.
4. Copy the code from below and paste it in your `live.yml` file

```jsx
name: Deploy to live

on:
  push:
    branches: [live]

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v2

      - uses: actions/setup-node@v1.4.4
        with:
          version: 16.x

      - name: Setup PHP with intl
        uses: shivammathur/setup-php@v2
        with:
          php-version: "8.1"
          extensions: intl-67.1

      - name: Install dependencies
        run: |
          composer install
          yarn install --ignore-engines
      - name: Build
        run: yarn build

      - name: Sync
        env:
          dest: "root@host_ip:/var/www/html/wp-content/themes/ThemeName"
        run: |
          echo "${{secrets.LIVE_DEPLOY_KEY}}" > deploy_key
          chmod 600 ./deploy_key
          rsync -chav --delete \
            -e 'ssh -i ./deploy_key -o StrictHostKeyChecking=no' \
            --exclude /deploy_key \
            --exclude /.git/ \
            --exclude /.github/ \
            --exclude /node_modules/ \
            ./ ${{env.dest}}
```

1. Inside jobs→deploy→steps→snyc, you will have to change the value of `dest`. This is what the value of dist will be for you `<user>@<host ip>:<path-to-your-theme>`. So for example `suraj@111.11.111.111:/var/www/html/wp-content/themes/theme-name`
2. After this is done you have to on "*Start Commit*" button and merge it into your master branch.
3. You will have to add a secret `LIVE_DEPLOY_KEY` which we will revisit after setting up our host.

# **2. Setting up your host machine**

`ssh` into your machine. We will create a SSH key and add it our github repository in order for our give our github repository access to our host machine.

We do this in the following steps.

```
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```

> Don't forget to change the email ID to yours. This will give you some prompts.
> 

```
> Generating public/private rsa key pair.
> Enter a file in which to save the key (/home/you/.ssh/id_rsa): [Press enter]
> Enter passphrase (empty for no passphrase): [Type a passphrase]
> Enter same passphrase again: [Type passphrase again]
```

You have to press enter and skip all of the above. In some cases you may already have an SSH key and you can overwrite it in order to create a fresh one.

You have now created a new ssh at `~/.ssh/id_rsa`.

### Adding SSH key to the ssh-agent

Enter the following command.

```
eval $(ssh-agent -s)
> Agent pid 59566
ssh-add ~/.ssh/id_rsa
```

Now we need to add our public key `id_rsa.pub` to the authorized_keys.

You can do this by.

```
# Getting inside SSH file
$ cd ~/.ssh/

# Copying pub key into your clipboard
$ cat id_rsa.pub
# This would print the content on the screen which you have to copy

# paste the content inside authorized_keys
$ nano authorized_keys
# To save press Ctrl+X -> y -> Press Enter

# Copy the content of public key
$ cat id_rsa
# This will paste the content of the public key which you have to copy starting from Start here till the end.
```

### Add secret to Github

In your Github repository you go → settings tab → Secrets & Variables → Actions → Click on `New repository secret` -> Type the name of the secret as `LIVE_DEPLOY_KEY` -> Paste the content of public key copied above here and hit `Add secret`.

We are almost done. We just have to test if everything is working fine or not.

## 3. Command to push code to Host from local*

Go to your local development and git push all the changes you want to deploy. When you are satisfied with the changes and you have pushed those in your master branch use this command.

```
git push origin main:live
```

This will push all the contents of master branch to production branch.

Now if you look closely at our `live.yml` file you can see these lines of code.

```
name: Deploy to live
on:
  push:
    branches: [ live ]

```

These lines tells that this action gets triggered when there is a push in the production branch.

You can look at your progress by going to the `Actions` tab. You would see an action is running. You can click on deploy to check the status and log of the same.

**[*Note: If you don’t have local code setup, You can manually create a new branch `live` on github and merge the `main` branch with `live` ]**