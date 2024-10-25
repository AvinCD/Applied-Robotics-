import robot
import time


def counter(a, R, G, B, summary):
    count = 0
    if a != count:
        count = a
        # Debug: print(a)
        print(R, G, B)

        if R > 200 and G < 100 and B < 100:
            print("Found Red")
            summary["Red"] += 1
        
        elif R < 100 and G > 150 and B < 100:
            print("Found Green")
            summary["Green"] += 1
        
        elif R < 100 and G < 100 and B > 150:
            print("Found Blue")
            summary["Blue"] += 1

        print("Summary of encountered blocks:", summary)

    else: 
        pass


def main():
    red = 0
    green = 0
    blue = 0
    range = 0.0
    past_duration = 0
    robot1 = robot.ARAP()
    robot1.init_devices()    
    
    summary = {"Red": 0, "Green": 0, "Blue": 0}  # Dictionary to store encountered counts

    start_time = time.time()
    while True: 
        end_time = time.time()
        duration = int(end_time - start_time)
        robot1.reset_actuator_values()
        
        range = robot1.get_sensor_input()
        robot1.blink_leds()
        red, green, blue = robot1.get_camera_image(5)

        if duration > past_duration:
            counter(duration, red, green, blue, summary)
        past_duration = duration
        
        if robot1.front_obstacles_detected():
            robot1.move_backward()
            robot1.turn_left()
        else:
            robot1.run_braitenberg()
        
        robot1.set_actuators()
        robot1.step()

if __name__ == "__main__":
    main()
