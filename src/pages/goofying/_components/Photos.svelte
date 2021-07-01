<script>
    import FilePond, { registerPlugin, supported } from "svelte-filepond";
    import FilePondPluginImageExifOrientation from "filepond-plugin-image-exif-orientation";
	import FilePondPluginImagePreview from "filepond-plugin-image-preview";
    import firebase from 'firebase/app';
    import { storage } from '../firebase';
    import { onMount } from "svelte";

    let pond;

    export let name = 'filepond';
    export let maxFiles = 1;
    let allowDrop = true;
    let server = {
        process:(fieldName, file, metadata, load, error, progress, about) => {
            const task = storage.child(`public/app/images/${file.name}`).put(file, {
                contentType: 'image/jpeg'
            });

            task.on(
                firebase.storage.TaskEvent.STATE_CHANGED,
                snap => progress(true, snap.bytesTransferred, snap.totalBytes),
                err => error(err.message),
                () => load(file.name)
            );
        },
        load:(source, load, error, progress, abort) => {
            progress(true, 0, 1024);

            storage.child(`public/app/images/${source}`)
                    .getDownloadURL()
                    .then(url => {
                        let xhr = new XMLHttpRequest();
                        xhr.responseType = 'blob';
                        xhr.onload = (event) => {
                            let blob = xhr.response;
                            load(blob);
                        };
                        xhr.open('GET', url);
                        xhr.send();
                    })
                    .catch(err => {
                        error(err.message);
                        abort();
                    });
        }
    };

    function handleInit(){
        console.log('Filepond has initialized');
    }

    function handleAddFile(err, fileItem){
        if(err) throw err;

        console.log('File has been added ', fileItem);
    }
    
    onMount(() => {
        registerPlugin(FilePondPluginImageExifOrientation, FilePondPluginImagePreview);
    });
</script>

<style global>
    @import 'filepond-plugin-image-preview/dist/filepond-plugin-image-preview.css';
    @import 'filepond/dist/filepond.min.css';
</style>

<div class="container">
    <FilePond
        bind:this={pond}
        {name}
        {server}
        {maxFiles}
        {allowDrop}
        allowMultiple={true}
        oninit={handleInit}
        onaddfile={handleAddFile}
    />
</div>