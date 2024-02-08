# Asusd

## Preferred configuration

This is the suggested config for asusd -- /etc/asusd/asusd.ron

```
(
    charge_control_end_threshold: 100,
    panel_od: false,
    mini_led_mode: false,
    disable_nvidia_powerd_on_battery: true,
    ac_command: "",
    bat_command: "",
    platform_policy_linked_epp: true,
    platform_policy_on_battery: Balanced,
    platform_policy_on_ac: Performance,
    ppt_pl1_spl: None,
    ppt_pl2_sppt: None,
    ppt_fppt: None,
    ppt_apu_sppt: None,
    ppt_platform_sppt: None,
    nv_dynamic_boost: None,
    nv_temp_target: None,
)
```

those who encounters strange behaviour(s) switching to battery can try the following:

```
(
    charge_control_end_threshold: 100,
    panel_od: false,
    mini_led_mode: false,
    disable_nvidia_powerd_on_battery: true,
    ac_command: "ryzenadj --max-performance",
    bat_command: "ryzenadj --max-performance",
    platform_policy_linked_epp: true,
    platform_policy_on_battery: Balanced,
    platform_policy_on_ac: Performance,
    ppt_pl1_spl: None,
    ppt_pl2_sppt: None,
    ppt_fppt: None,
    ppt_apu_sppt: None,
    ppt_platform_sppt: None,
    nv_dynamic_boost: None,
    nv_temp_target: None,
)
```