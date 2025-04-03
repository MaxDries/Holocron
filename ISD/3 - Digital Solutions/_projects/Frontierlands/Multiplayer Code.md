This code was removed from the original project. Include it again in the indicated files to reenable the functionality.

# The level scene

For example `testWorld.tscn`.

This requires editing the file in a text editor to add the nodes. 

```
[node name="HostButton" type="Button" parent="CanvasLayer/MainMenu/MarginContainer/VBoxContainer"]
visible = false
layout_mode = 2
text = "Host"
[node name="JoinButton" type="Button" parent="CanvasLayer/MainMenu/MarginContainer/VBoxContainer"]
visible = false
layout_mode = 2
text = "Join"
[node name="AddressEntry" type="LineEdit" parent="CanvasLayer/MainMenu/MarginContainer/VBoxContainer"]
visible = false
layout_mode = 2
placeholder_text = "Enter Address to Join Here"
alignment = 1


[connection signal="pressed" from="CanvasLayer/MainMenu/MarginContainer/VBoxContainer/HostButton" to="." method="_on_host_button_pressed"]
[connection signal="pressed" from="CanvasLayer/MainMenu/MarginContainer/VBoxContainer/JoinButton" to="." method="_on_join_button_pressed"]
```


# `world.gd`

```gdscript

const PORT = 9999
var enet_peer = ENetMultiplayerPeer.new()


func _on_host_button_pressed():
	main_menu.hide()
	hud.show()
	
	enet_peer.create_server(PORT)
	multiplayer.multiplayer_peer = enet_peer
	multiplayer.peer_connected.connect(add_player)
	multiplayer.peer_disconnected.connect(remove_player)
	
	add_player(multiplayer.get_unique_id())
	
	#upnp_setup()
func _on_join_button_pressed():
	main_menu.hide()
	hud.show()
	
	enet_peer.create_client(address_entry.text, PORT)
	multiplayer.multiplayer_peer = enet_peer

func _on_multiplayer_spawner_spawned(node):
	if node.is_multiplayer_authority():
		node.health_changed.connect(update_health_bar)
func upnp_setup():
	var upnp = UPNP.new()
	
	var discover_result = upnp.discover()
	assert(discover_result == UPNP.UPNP_RESULT_SUCCESS, "UPNP Discover Failed! Error %s" % discover_result)

	assert(upnp.get_gateway() and upnp.get_gateway().is_valid_gateway(), "UPNP Invalid Gateway!")

	var map_result = upnp.add_port_mapping(PORT)
	assert(map_result == UPNP.UPNP_RESULT_SUCCESS, "UPNP Port Mapping Failed! Error %s" % map_result)
	
	print("Success! Join Address: %s" % upnp.query_external_address())


```

