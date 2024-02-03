# Base64 to File

Utilize this action if you need to retrieve a file from a base64-encoded string that you may be storing in Secrets or elsewhere. This can be useful for certificate signing and storing the base64 cert in the Secrets.

## 1. Usage

   ```yaml
    - name: Run Workflow
      id: write_base64_file
      uses: Swisyn/base64-hash-to-file@v1
      with:
        destinationFileName: 'myTemporaryBase64File.txt'
        destinationPath: './main/folder/'
        encodedString: ${{ secrets.ENCODED_BASE64_STRING }}
   ```

By default, this function writes the `destinationFileName` to a temporary path defined by env.RUNNER_TEMP. If you prefer a different writable path, you can specify `destinationPath` as an input argument. In this case, `destinationPath` and `destinationFileName` will be merged to form the path where the output will be saved. It's important to note that this operation assumes correct permissions in the `destinationPath` and does not attempt to modify them.

## 2. Using the output

The Action provides an output variable called `filePath`, which you can utilize as the file is written to TEMP. Ensure you include an id in your step when employing this Action, enabling easy retrieval from the steps context later on.

   ```yaml
    - name: Run Workflow
      id: write_base64_file
      uses: Swisyn/base64-hash-to-file@v1
      with:
        destinationFileName: 'myTemporaryBase64File.txt'
        encodedString: ${{ secrets.ENCODED_BASE64_STRING }}

    - name: Next step
      uses: actions/someaction@v1
      with:
          filelocation: ${{ steps.write_base64_file.outputs.filePath }}
   ```

