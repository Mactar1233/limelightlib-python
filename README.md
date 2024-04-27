# LimelightLib Python

## Discover all connected Limelights, and interact with them via REST and Websockets

```
import limelight
import limelightresults

discovered_limelights = limelight.discover_limelights(debug=True)
print("discovered limelights:", discovered_limelights)

if discovered_limelights:
    limelight_address = discovered_limelights[0] 
    ll = limelight.Limelight(limelight_address)
    results = ll.get_results()
    status = ll.get_status()
    print("-----")
    print("targeting results:", results)
    print("-----")
    print("status:", status)
    print("-----")
    print("temp:", ll.get_temp())
    print("-----")
    print("name:", ll.get_name())
    print("-----")
    print("fps:", ll.get_fps())

    ll.enable_websocket()

    try:
        while True:
            result = ll.get_latest_results()
            parsed_result = limelightresults.parse_results(result)
            if parsed_result is not None:
                print("valid targets: ", parsed_result.validity, ", pipelineIndex: ", parsed_result.pipeline_id,", Targeting Latency: ", parsed_result.targeting_latency)
                #for tag in parsed_result.fiducialResults:
                #    print(tag.robot_pose_target_space, tag.fiducial_id)

    except KeyboardInterrupt:
        print("Program interrupted by user, shutting down.")
    finally:
        ll.disable_websocket()

```
