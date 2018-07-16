# After Installation, the config is still not displayed #
Please try to do the installation steps again. Check if your version requirements hit the requirements in the "Before you start section". Check if the Leagua/Flysystem module is there in the vendor folder and try to flush the cache again.

If the problem still persists, please open an issue, so other people can help you.

# The Folders are displayed but WhereTF are my symlinks?? #

In the local adapter symlinks currently can't be displayed with flysystem. The SKIP_LINKS property given to the adapter creation means, that all links will be skipped.

If you change that property, Flysystem will return an error, that links can't be displayed. Thats not an issue about the Magento2 module but the Flysystem module itself.

If you want to get further information about this, you can find some f.e. here:
[Issue 599](https://github.com/thephpleague/flysystem/issues/599) or [Issue 203](https://github.com/thephpleague/flysystem/issues/203)