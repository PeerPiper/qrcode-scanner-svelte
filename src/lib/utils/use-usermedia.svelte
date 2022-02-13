<script lang="ts">
	import { stream, error, status } from '../stores.js';

	const isMediaStream = (
		candidate: MediaStream | MediaSource | Blob | null
	): candidate is MediaStream => candidate !== null && 'getTracks' in candidate;

	type UseUserMediaStatusType = 'pending' | 'resolved' | 'rejected' | 'stopped';

	interface UseUserMediaType {
		stopMediaStream: () => void;
		startMediaStream: () => void;
	}

	function setStatus(params: string) {
		console.log(`Setting status ${params}`);

		$status = params;
	}

	export const useUserMedia = (): UseUserMediaType => {
		$stream = null;
		$error = null;
		$status = 'stopped';

		function setError(params: string) {
			console.log('Setting erro');
			$error = params;
		}

		function setStream(params: MediaStream) {
			console.log(`Setting stream`, { params });
			$stream = params;
		}

		const startMediaStream = (): void => {
			setStatus('pending');

			navigator.mediaDevices
				.getUserMedia({
					audio: false,
					video: {
						facingMode: 'environment'
					}
				})
				.then((userStream) => {
					setStream(userStream);
					setStatus('resolved');
				})
				.catch((err) => {
					setError(err);
					setStatus('rejected');
				});
		};

		const stopMediaStream = (): void => {
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
