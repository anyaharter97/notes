# Arbitrary Tasks

#### Create an Email Filter
1. Create a new folder on the Zimbra web interface
2. On Thunderbird, go to an email sent by the list for which you are creating the filter
3. Click **More > View Source**
4. Scan the source for the **list-id** tag and copy the address inside the < brackets >
5. Back on Zimbra, go to **Preferences > Filters > New Filter**
6. Change **Subject** to **Header Named** from the drop-down list and enter `list-id` into the field that appears
7. Paste the address you selected in step 4. into the **contains** field.
8. Change **Keep in Inbox** to **Move to Folder** and Browse for the folder you created in step 1. and then click OK
9. Click **Run Filter**
10. Check the box next to **Inbox** and then click **OK**
11. In Thunderbird, right click on the top most folder for your email account and then select **Subscribe** from the drop down menu
12. Check the box next to your new folder
13. Click **Subscribe**  and then OK
14. Right click on the new folder that appears in Thunderbird and select **Properties** from the drop down menu
15. Check the box next to **"When getting new messages for this account, always check this folder"** and then click OK

#### Add Linked File to "Config" Repo From Existing File
1. Run `cp <absolute-file-path> ~/bin/config/<no-dot-filename>` to copy the file into `config`
2. Add file to tracked on git and commit
3. Run `ln -sf ~/bin/config/<no-dot-filename> <absolute-file-path-with-dot>` to create the link
4. Run `ls -a` from their original location, the linked file should turn aqua
5. Now any changes you make to that file in it's original location will be mapped back to the file inside `config` and you can commit and push them from there

#### Getting Configure System File
1. The command `rmp --eval "%configure"` basically gives you the config stuff compatible with the distribution you're running
2. Copy and paste it into `~/bin/configuresystem.sh` with some extra stuff (below)
```sh
#!/bin/bash
exec ./configure --host=x86_64-redhat-linux-gnu --build=x86_64-redhat-linux-gnu \
>...--program-prefix= \
>...--disable-dependency-tracking \
>...--prefix=/usr \
>...--exec-prefix=/usr \
>...--bindir=/usr/bin \
>...--sbindir=/usr/sbin \
>...--sysconfdir=/etc \
>...--datadir=/usr/share \
>...--includedir=/usr/include \
>...--libdir=/usr/lib64 \
>...--libexecdir=/usr/libexec \
>...--localstatedir=/var \
>...--sharedstatedir=/var/lib \
>...--mandir=/usr/share/man \
>...--infodir=/usr/share/info "$@"
```
3. To make the shell script executable run `chmod +x ~/bin/configuresystem.sh`
4. Now, running `configuresystem.sh` essentially runs the configure file with a bunch of specified personalized instructions that are compatible with your distribution
