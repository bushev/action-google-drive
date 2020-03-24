# Upload to Google Drive

This action uploads the specified directory/files to Google Drive.

## Example

```yaml
- name: Upload to Google Drive
  uses: satackey/action-google-drive@v1
  with:
    skicka-tokencache-json: ${{ secrets.SKICKA_TOKENCACHE_JSON }}
    upload-from: ./
    upload-to: /path/to/upload
```

## Get ready 

This action uses [`skicka`](https://github.com/google/skicka) for uploading to Google Drive.
You need to generate token and register it with GitHub secrets.

### How to generate token

#### Users already using skicka

In your GitHub repository → Settings → Secrets, register by entering `SKICKA_TOKENCACHE_JSON` for name and the content of `~/.skicka.tokencache.json` for value.

#### Users who have never used `skicka`

1. Setup Docker and execute following command.
    ```sh
    docker run --rm -it --entrypoint "" satackey/skicka sh -c "skicka -no-browser-auth ls && cat /root/.skicka.tokencache.json"
    ```
1. Access the URL showed in the output
1. Grant access, paste code showed in the browser into the terminal.
1. In your GitHub repository → Settings → Secrets, register by entering `SKICKA_TOKENCACHE_JSON` for name and the content of `~/.skicka.tokencache.json` for value.

    ```json
    {"ClientId":"xxx-xxxxx.apps.googleusercontent.com","access_token":"xxxx.xx-xxxxxxxxx","token_type":"Bearer","refresh_token":"x//xxxxxxx-xxxxxxx","expiry":"2020-01-03T06:11:01.3298117Z"}
    ````

##### Troubleshooting of sign in

As of Jan. 2, 2020, for the accounts that sign in to skicka for the first time, may have a problem of being displayed as `Sign in with Google temporarily disabled for this app`.
The workaround is to set up the Google Drive API client ID and secret, and set them in skicka.

[Follow this article (japanese only)](https://qiita.com/satackey/items/34c7fc5bf77bd2f5c633) to set up a client ID and secret. [See Translated by Google](https://translate.google.com/translate?&sl=ja&tl=en&u=https%3A%2F%2Fqiita.com%2Fsatackey%2Fitems%2F34c7fc5bf77bd2f5c633)

Replace `xxxx-your-google-client-id-xx.googleusercontent.com` and `xxx_yourGoogleClientSecret_xxxx`, run the following command.

```shell
$ docker run -e GOOGLE_CLIENT_ID=xxxx-your-google-client-id-xx.googleusercontent.com -e GOOGLE_CLIENT_SECRET=xxx_yourGoogleClientSecret_xxxx --rm -it --entrypoint "ash" satackey/skicka
```

When the container starts, run the following command.
```
# sed -i -e "s/;clientid=YOUR_GOOGLE_APP_CLIENT_ID/clientid=$GOOGLE_CLIENT_ID/" ~/.skicka.config && sed -i -e "s/;clientsecret=YOUR_GOOGLE_APP_SECRET/clientsecret=$GOOGLE_CLIENT_SECRET/" ~/.skicka.config && skicka -no-browser-auth ls && cat /root/.skicka.tokencache.json
```

Return to step 2 and proceed.

## Inputs

- `skicka-tokencache-json` **Required**  
    The credentials of the account to upload, generated by `skicka`. (Contents of `~/.skicka.tokencache.json`)

- `upload-from` optional  
    Upload source path. Default is the current directory.

- `upload-to` optional  
    Upload destination path.

- `google-client-id` optional  
    OAuth2.0 client ID of Google APIs when using skicka.

- `google-client-secret` optional  
    OAuth2.0 Client Secret of Google APIs when using skicka.

- `remove-outdated` optional  
    Whether to delete files that are not local but exist on Google Drive, either `'true'` or`' false'`  
    > **Note**: It is recommended to turn it off when performing operations involving large files, because it detects files that do not exist locally and downloads them.

## Contribution
PRs are accepted.

If you are having trouble or future request, [post new issue](https://github.com/satackey/action-google-drive/issues/new).
