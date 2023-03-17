<script lang="ts">
    import { stream, error, status } from '../stores';
    const isMediaStream = (candidate) => candidate !== null && 'getTracks' in candidate;
    function setStatus(params) {
        console.log(`Setting status ${params}`);
        $status = params;
    }
export const useUserMedia = () => {
    $stream = null;
    $error = null;
    $status = 'stopped';
    function setError(params) {
        console.log('Setting erro');
        $error = params;
    }
    function setStream(params) {
        console.log(`Setting stream`, { params });
        $stream = params;
    }
    // const startMediaStream = () => {
    //     setStatus('pending');
    //     navigator.mediaDevices.getUserMedia({
    //             audio: false,
    //             video: {
    //                 facingMode: 'environment'
    //             }
    //     }).then((userStream) => {
    //             setStream(userStream);
    //             setStatus('resolved');
    //     }).catch((err) => {
    //             setError(err);
    //             setStatus('rejected');
    //     });
    // };
    const startMediaStream = async ()=>{
        try {
            setStatus('pending');
            let stream = await navigator.mediaDevices.getUserMedia({
                audio: false,
                video: {facingMode: 'environment'}
            });
            setStream(stream);
            setStatus('resolved');
        }catch (e) {
            setError(e);
            setStatus('rejected');
            console.error('Error accessing camera:', e);
        }
    }
    const stopMediaStream = () => {
        console.log('stopping media stream');
        if (isMediaStream($stream)) {
            $stream.getTracks().forEach((track) => {
                track.stop();
                $stream.removeTrack(track);
            });
            setStatus('stopped');
        }
    };
    return { stopMediaStream, startMediaStream };
};
</script>
