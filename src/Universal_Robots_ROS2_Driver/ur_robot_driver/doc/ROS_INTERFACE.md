**NOTE**: Esta documentação está obsoleta e não cobre totalmente o estado atual do driver.

# ROS interface

O novo driver para robôs Universal Robots UR3, UR5 e UR10 com controladores CB3 e a série e.

## Nodes

### ur_ros2_control_node

Este é o nó de driver real que contém a pilha ROS2-Control. As interfaces documentadas aqui se referem à interface de hardware do robô. Elementos de API específicos do controlador podem estar presentes para os controladores individuais fora deste pacote.

#### Parameters
Observe que os parâmetros são passados ​​através da definição ros2_control xacro.

##### headless_mode

Inicie o robô no modo headless. Isso não requer que o URCap 'External Control' esteja em execução no robô, mas isso enviará o URScript diretamente para o robô. Em robôs e-Series, isso requer que o robô seja executado no modo 'remote-control'.

##### input_recipe_filename (Required)

Caminho para o arquivo que contém a receita usada para solicitar entradas RTDE.

##### kinematics/hash (Required)

Hash da calibração relatada pelo robô. Isso é usado para validar se a descrição do robô está usando a calibração correta. Se a calibração do robô não corresponder a esse hash, um erro será impresso. Você pode usar o robô normalmente, no entanto, as poses cartesianas do endeffector podem ser imprecisas. Veja o pacote "ur_calibration" na ajuda sobre como gerar seu próprio hash correspondente ao seu robô real.

##### non_blocking_read (default: "false")

Habilita o modo non_blocking_read. Desabilita o erro gerado quando a leitura retorna sem nenhum dado, define
o tempo limite de leitura para zero e sincroniza as operações de leitura/gravação. Isso é um resquício do driver ROS1
e atualmente não vemos nenhuma aplicação real onde faria sentido habilitar isso.

##### output_recipe_filename (Required)

Caminho para o arquivo que contém a receita usada para solicitar saídas RTDE.

##### reverse_port (Required)

Port that will be opened to communicate between the driver and the robot controller.

##### robot_ip (Required)

The robot's IP address.

##### script_filename (Required)

Path to the urscript code that will be sent to the robot.

##### script_sender_port (Required)

The driver will offer an interface to receive the program's URScript on this port.

##### servoj_gain (Required)

Specify the gain for the underlying servoj command. This will be used whenever position control is
active. A higher value will lead to sharper motions, but might also introduce
higher jerks and vibrations.

Range: [100 - 3000]

##### servoj_lookahead_time (Required)

Specify lookahead_time parameter of underlying servoj command. This will be used whenever position
control is active. A higher value will result in smoother trajectories, but will also introduce a
higher delay between the commands sent from ROS and the motion being executed on the robot.

Unit: seconds, range: [0.03 - 0.2]



##### tool_baud_rate (Required)

Baud rate used for tool communication. Will be set as soon as the UR-Program on the robot is started. See UR documentation for valid baud rates.  Note: This parameter is only evaluated, when the parameter "use_tool_communication" is set to TRUE.  Then, this parameter is required.

##### tool_parity (Required)

Parity used for tool communication. Will be set as soon as the UR-Program on the robot is started. Can be 0 (None), 1 (odd) and 2 (even).  Note: This parameter is only evaluated, when the parameter "use_tool_communication" is set to TRUE.  Then, this parameter is required.

##### tool_rx_idle_chars (Required)

Number of idle chars for the RX unit used for tool communication. Will be set as soon as the UR-Program on the robot is started. Valid values: min=1.0, max=40.0  Note: This parameter is only evaluated, when the parameter "use_tool_communication" is set to TRUE.  Then, this parameter is required.

##### tool_stop_bits (Required)

Number of stop bits used for tool communication. Will be set as soon as the UR-Program on the robot is started. Can be 1 or 2.  Note: This parameter is only evaluated, when the parameter "use_tool_communication" is set to TRUE.  Then, this parameter is required.

##### tool_tx_idle_chars (Required)

Number of idle chars for the TX unit used for tool communication. Will be set as soon as the UR-Program on the robot is started. Valid values: min=0.0, max=40.0  Note: This parameter is only evaluated, when the parameter "use_tool_communication" is set to TRUE.  Then, this parameter is required.

##### tool_voltage (Required)

Tool voltage that will be set as soon as the UR-Program on the robot is started. Note: This parameter is only evaluated, when the parameter "use_tool_communication" is set to TRUE. Then, this parameter is required.

##### use_tool_communication (Required)

Should the tool's RS485 interface be forwarded to the ROS machine? This is only available on e-Series models. Setting this parameter to TRUE requires multiple other parameters to be set,as well.


### dashboard_client

#### Advertised Services

##### add_to_log ([ur_dashboard_msgs/AddToLog](http://docs.ros.org/api/ur_dashboard_msgs/html/srv/AddToLog.html))

Service to add a message to the robot's log

##### brake_release ([std_srvs/Trigger](http://docs.ros.org/api/std_srvs/html/srv/Trigger.html))

Service to release the brakes. If the robot is currently powered off, it will get powered on on the fly.

##### clear_operational_mode ([std_srvs/Trigger](http://docs.ros.org/api/std_srvs/html/srv/Trigger.html))

If this service is called the operational mode can again be changed from PolyScope, and the user password is enabled.

##### close_popup ([std_srvs/Trigger](http://docs.ros.org/api/std_srvs/html/srv/Trigger.html))

Close a (non-safety) popup on the teach pendant.

##### close_safety_popup ([std_srvs/Trigger](http://docs.ros.org/api/std_srvs/html/srv/Trigger.html))

Close a safety popup on the teach pendant.

##### connect ([std_srvs/Trigger](http://docs.ros.org/api/std_srvs/html/srv/Trigger.html))

Service to reconnect to the dashboard server

##### get_loaded_program ([ur_dashboard_msgs/GetLoadedProgram](http://docs.ros.org/api/ur_dashboard_msgs/html/srv/GetLoadedProgram.html))

Load a robot installation from a file

##### get_robot_mode ([ur_dashboard_msgs/GetRobotMode](http://docs.ros.org/api/ur_dashboard_msgs/html/srv/GetRobotMode.html))

Service to query the current robot mode

##### get_safety_mode ([ur_dashboard_msgs/GetSafetyMode](http://docs.ros.org/api/ur_dashboard_msgs/html/srv/GetSafetyMode.html))

Service to query the current safety mode

##### load_installation ([ur_dashboard_msgs/Load](http://docs.ros.org/api/ur_dashboard_msgs/html/srv/Load.html))

Load a robot installation from a file

##### load_program ([ur_dashboard_msgs/Load](http://docs.ros.org/api/ur_dashboard_msgs/html/srv/Load.html))

Load a robot program from a file

##### pause ([std_srvs/Trigger](http://docs.ros.org/api/std_srvs/html/srv/Trigger.html))

Pause a running program.

##### play ([std_srvs/Trigger](http://docs.ros.org/api/std_srvs/html/srv/Trigger.html))

Start execution of a previously loaded program

##### popup ([ur_dashboard_msgs/Popup](http://docs.ros.org/api/ur_dashboard_msgs/html/srv/Popup.html))

Service to show a popup on the UR Teach pendant.

##### power_off ([std_srvs/Trigger](http://docs.ros.org/api/std_srvs/html/srv/Trigger.html))

Power off the robot motors

##### power_on ([std_srvs/Trigger](http://docs.ros.org/api/std_srvs/html/srv/Trigger.html))

Power on the robot motors. To fully start the robot, call 'brake_release' afterwards.

##### program_running ([ur_dashboard_msgs/IsProgramRunning](http://docs.ros.org/api/ur_dashboard_msgs/html/srv/IsProgramRunning.html))

Query whether there is currently a program running

##### program_saved ([ur_dashboard_msgs/IsProgramSaved](http://docs.ros.org/api/ur_dashboard_msgs/html/srv/IsProgramSaved.html))

Query whether the current program is saved

##### program_state ([ur_dashboard_msgs/GetProgramState](http://docs.ros.org/api/ur_dashboard_msgs/html/srv/GetProgramState.html))

Service to query the current program state

##### quit ([ur_dashboard_msgs/GetLoadedProgram](http://docs.ros.org/api/ur_dashboard_msgs/html/srv/GetLoadedProgram.html))

Disconnect from the dashboard service.

##### raw_request ([ur_dashboard_msgs/RawRequest](http://docs.ros.org/api/ur_dashboard_msgs/html/srv/RawRequest.html))

General purpose service to send arbitrary messages to the dashboard server

##### restart_safety ([std_srvs/Trigger](http://docs.ros.org/api/std_srvs/html/srv/Trigger.html))

Used when robot gets a safety fault or violation to restart the safety. After safety has been rebooted the robot will be in Power Off. NOTE: You should always ensure it is okay to restart the system. It is highly recommended to check the error log before using this command (either via PolyScope or e.g. ssh connection).

##### shutdown ([std_sPath to the file containing the recipe used for requesting RTDE outputs.rvs/Trigger](http://docs.ros.org/api/std_srvs/html/srv/Trigger.html))

Shutdown the robot controller

##### stop ([std_srvs/Trigger](http://docs.ros.org/api/std_srvs/html/srv/Trigger.html))

Stop program execution on the robot

##### unlock_protective_stop ([std_srvs/Trigger](http://docs.ros.org/api/std_srvs/html/srv/Trigger.html))

Dismiss a protective stop to continue robot movements. NOTE: It is the responsibility of the user to ensure the cause of the protective stop is resolved before calling this service.

#### Parameters

##### receive_timeout (Required)

Timeout after which a call to the dashboard server will be considered failure if no answer has been received.

##### robot_ip (Required)

The IP address under which the robot is reachable.
