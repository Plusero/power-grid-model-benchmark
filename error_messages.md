# Error Message 1
```
---------------------------------------------------------------------------
IndexError                                Traceback (most recent call last)
Cell In[6], line 1
----> 1 fictional_dataset = generate_fictional_grid(
      2     n_node_per_feeder=3,
      3     n_feeder=2,
      4     cable_length_km_min=cable_length_km_min,

File ~/prjs_my/power-grid-model-benchmark/generate_fictional_dataset.py:528, in generate_fictional_grid(n_feeder, n_node_per_feeder, cable_length_km_min, cable_length_km_max, load_p_w_max, load_p_w_min, pf, n_step, load_scaling_min, load_scaling_max, seed)
    518     output_file.write(output)
    520 # return values
    521 return {
    522     "pgm_dataset": pgm_dataset,
    523     "pp_net": pp_net,
    524     "pgm_update_dataset": {"asym_load": asym_load_profile},
    525     "pp_time_series_dataset": pp_dataset,
    526     "pp_time_series_dataset_sym": pp_dataset_sym,
    527     "dss_file": output_path,
--> 528     "l2g_input": LightSim2GridNetInput.from_pandapower_net(pp_net),
    529     "g2o_input": GRID2OP_PATH / "grid.json",
    530     "g2o_update": {
    531         "load_p": GRID2OP_PATH / "load_p.csv",
    532         "load_q": GRID2OP_PATH / "load_q.csv",
    533     },
    534 }

File ~/prjs_my/power-grid-model-benchmark/generate_fictional_dataset.py:139, in LightSim2GridNetInput.from_pandapower_net(pp_net)
    136 v0 = bus[:, VM] * np.exp(1j * np.pi / 180.0 * bus[:, VA])
    137 v0[gbus] = gen[on, VG] / abs(v0[gbus]) * v0[gbus]
--> 139 Ybus, _, _ = makeYbus(baseMVA, bus, branch)
    140 Sbus = makeSbus(baseMVA, bus, gen)
    142 return LightSim2GridNetInput(
    143     Ybus=Ybus.tocsc(),
    144     Sbus=Sbus,
   (...)    149     ppci=ppci,
    150 )

File ~/prjs_my/power-grid-model-benchmark/.venv/lib/python3.14/site-packages/pandapower/pf/makeYbus_numba.py:142, in makeYbus(baseMVA, bus, branch)
    134 nl = branch.shape[0]  ## number of lines
    136 ## for each branch, compute the elements of the branch admittance matrix where
    137 ##
    138 ##      | If |   | Yff  Yft |   | Vf |
    139 ##      |    | = |          | * |    |
    140 ##      | It |   | Ytf  Ytt |   | Vt |
    141 ##
--> 142 Ytt, Yff, Yft, Ytf = branch_vectors(branch, nl)
    144 ## compute shunt admittance
    145 ## if Psh is the real power consumed by the shunt at V = 1.0 p.u.
    146 ## and Qsh is the reactive power injected by the shunt at V = 1.0 p.u.
    147 ## then Psh - j Qsh = V * conj(Ysh * V) = conj(Ysh) = Gs - j Bs,
    148 ## i.e. Ysh = Psh + j Qsh, so ...
    149 ## vector of shunt admittances
    150 Ysh = (bus[:, GS] + 1j * bus[:, BS]) / baseMVA

File ~/prjs_my/power-grid-model-benchmark/.venv/lib/python3.14/site-packages/numpy/_core/_ufunc_config.py:511, in errstate.__call__.<locals>.inner(*args, **kwargs)
    508 _token = _extobj_contextvar.set(extobj)
    509 try:
    510     # Call the original, decorated, function:
--> 511     return func(*args, **kwargs)
    512 finally:
    513     _extobj_contextvar.reset(_token)

File ~/prjs_my/power-grid-model-benchmark/.venv/lib/python3.14/site-packages/pandapower/pypower/makeYbus.py:90, in branch_vectors(branch, nl)
     88 stat = branch[:, BR_STATUS]  # ones at in-service branches
     89 Ysf = stat / (branch[:, BR_R] + 1j * branch[:, BR_X])  # series admittance
---> 90 if any(branch[:, BR_R_ASYM]) or any(branch[:, BR_X_ASYM]):
     91     Yst = stat / (branch[:, BR_R] + branch[:, BR_R_ASYM] +
     92                   1j * (branch[:, BR_X] + branch[:, BR_X_ASYM]))
     93 else:

IndexError: index 22 is out of bounds for axis 1 with size 22
```

## Error Message 2
---------------------------------------------------------------------------
KeyError                                  Traceback (most recent call last)
File ~/prjs_my/power-grid-model-benchmark/.venv/lib/python3.14/site-packages/pandas/core/indexes/base.py:3812, in Index.get_loc(self, key)
   3811 try:
-> 3812     return self._engine.get_loc(casted_key)
   3813 except KeyError as err:

File pandas/_libs/index.pyx:167, in pandas._libs.index.IndexEngine.get_loc()
--> 167 'Could not get source, probably due dynamically evaluated source code.'

File pandas/_libs/index.pyx:196, in pandas._libs.index.IndexEngine.get_loc()
--> 196 'Could not get source, probably due dynamically evaluated source code.'

File pandas/_libs/hashtable_class_helper.pxi:7088, in pandas._libs.hashtable.PyObjectHashTable.get_item()
-> 7088 'Could not get source, probably due dynamically evaluated source code.'

File pandas/_libs/hashtable_class_helper.pxi:7096, in pandas._libs.hashtable.PyObjectHashTable.get_item()
-> 7096 'Could not get source, probably due dynamically evaluated source code.'

KeyError: 'const_z_p_percent'

The above exception was the direct cause of the following exception:

KeyError                                  Traceback (most recent call last)
Cell In[13], line 32
     28     pp_backend.apply_action.reset()
     29     pp_backend.runpf.reset()
     30 
     31 
---> 32 benchmark_with_other_timer_and_return_result(
     33     run_g2o_pp,
     34     timer=time_g2o_pp,
     35     setup=setup_g2o_pp,

Cell In[9], line 53, in benchmark_with_other_timer_and_return_result(func, timer, method, calculation, is_batch, setup)
     49     calculation: Calculation,
     50     is_batch: bool,
     51     setup="pass",
     52 ):
---> 53     execution = benchmark_return_time_and_result(
     54         func=func, is_batch=is_batch, setup=setup
     55     )
     56 

Cell In[9], line 19, in benchmark_return_time_and_result(func, is_batch, setup)
     15     def _func():
     16         nonlocal result
     17         result = func()
     18 
---> 19     execution_time = timeit.repeat(
     20         _func,
     21         setup=setup,
     22         repeat=1 if is_batch else n_single_scenario_repeats,

File ~/.local/share/uv/python/cpython-3.14.3-linux-x86_64-gnu/lib/python3.14/timeit.py:240, in repeat(stmt, setup, timer, repeat, number, globals)
    237 def repeat(stmt="pass", setup="pass", timer=default_timer,
    238            repeat=default_repeat, number=default_number, globals=None):
    239     """Convenience function to create Timer object and call repeat method."""
--> 240     return Timer(stmt, setup, timer, globals).repeat(repeat, number)

File ~/.local/share/uv/python/cpython-3.14.3-linux-x86_64-gnu/lib/python3.14/timeit.py:205, in Timer.repeat(self, repeat, number)
    203 r = []
    204 for i in range(repeat):
--> 205     t = self.timeit(number)
    206     r.append(t)
    207 return r

File ~/.local/share/uv/python/cpython-3.14.3-linux-x86_64-gnu/lib/python3.14/timeit.py:177, in Timer.timeit(self, number)
    175 gc.disable()
    176 try:
--> 177     timing = self.inner(it, self.timer)
    178 finally:
    179     if gcold:

File <timeit-src>:6, in inner(_it, _timer, _setup, _stmt)
      2 'Could not get source, probably due dynamically evaluated source code.'

Cell In[9], line 17, in benchmark_return_time_and_result.<locals>._func()
     15     def _func():
     16         nonlocal result
---> 17         result = func()

Cell In[13], line 14, in run_g2o_pp()
     12 def run_g2o_pp():
     13     global g2o_pp_grid
---> 14     g2o_pp_grid = g2o.make(
     15         Path("g2o_grid_sym").absolute(),
     16         backend=pp_backend,
     17         data_feeding_kwargs={

File ~/prjs_my/power-grid-model-benchmark/.venv/lib/python3.14/site-packages/grid2op/MakeEnv/Make.py:429, in make(dataset, test, logger, experimental_read_from_local_dir, n_busbar, allow_detachment, _add_cls_nm_bk, _add_to_name, _compat_glop_version, _overload_name_multimix, **kwargs)
    424     if not "experimental_read_from_local_dir" in kwargs:
    425         kwargs[
    426             "experimental_read_from_local_dir"
    427         ] = experimental_read_from_local_dir
--> 429     return make_from_path_fn(
    430         dataset_path=dataset,
    431         _add_cls_nm_bk=_add_cls_nm_bk,
    432         _add_to_name=_add_to_name_tmp,
    433         _compat_glop_version=_compat_glop_version_tmp,
    434         _overload_name_multimix=_overload_name_multimix,
    435         n_busbar=n_busbar,
    436         allow_detachment=allow_detachment,
    437         **kwargs
    438     )
    440 # Not a path: get the dataset name and cache path
    441 dataset_name = _extract_ds_name(dataset)

File ~/prjs_my/power-grid-model-benchmark/.venv/lib/python3.14/site-packages/grid2op/MakeEnv/MakeFromPath.py:1065, in make_from_dataset_path(dataset_path, logger, experimental_read_from_local_dir, n_busbar, allow_detachment, _add_cls_nm_bk, _add_to_name, _compat_glop_version, _overload_name_multimix, **kwargs)
   1061         classes_path = this_local_dir.name
   1063 # Finally instantiate env from config & overrides
   1064 # including (if activated the new grid2op behaviour)
-> 1065 env = Environment(
   1066     **default_kwargs,
   1067      chronics_handler=data_feeding,
   1068     _allow_loaded_backend=allow_loaded_backend,
   1069     _read_from_local_dir=classes_path,
   1070     _local_dir_cls=this_local_dir,
   1071 )   
   1072 if do_not_erase_cls is not None:
   1073     env._do_not_erase_local_dir_cls = do_not_erase_cls

File ~/prjs_my/power-grid-model-benchmark/.venv/lib/python3.14/site-packages/grid2op/Environment/environment.py:223, in Environment.__init__(self, init_env_path, init_grid_path, chronics_handler, backend, parameters, name, n_busbar, allow_detachment, names_chronics_to_backend, actionClass, observationClass, rewardClass, legalActClass, voltagecontrolerClass, other_rewards, thermal_limit_a, with_forecast, epsilon_poly, tol_poly, opponent_space_type, opponent_action_class, opponent_class, opponent_init_budget, opponent_budget_per_ts, opponent_budget_class, opponent_attack_duration, opponent_attack_cooldown, kwargs_opponent, attention_budget_cls, kwargs_attention_budget, has_attention_budget, logger, kwargs_observation, observation_bk_class, observation_bk_kwargs, highres_sim_counter, _update_obs_after_reward, _init_obs, _raw_backend_class, _compat_glop_version, _read_from_local_dir, _is_test, _allow_loaded_backend, _local_dir_cls, _overload_name_multimix)
    220 self._observationClass_orig = observationClass
    222 # init the backend
--> 223 self._init_backend(
    224     chronics_handler,
    225     backend,
    226     names_chronics_to_backend,
    227     actionClass,
    228     observationClass,
    229     rewardClass,
    230     legalActClass,
    231 )

File ~/prjs_my/power-grid-model-benchmark/.venv/lib/python3.14/site-packages/grid2op/Environment/environment.py:307, in Environment._init_backend(self, chronics_handler, backend, names_chronics_to_backend, actionClass, observationClass, rewardClass, legalActClass)
    304 if self._compat_glop_version is not None:
    305     type(self.backend).glop_version = self._compat_glop_version
--> 307 self.backend.load_grid_public(
    308     self._init_grid_path
    309 )  # the real powergrid of the environment
    310 self.backend.load_storage_data(self.get_path_env())
    311 self.backend._fill_names_obj()

File ~/prjs_my/power-grid-model-benchmark/.venv/lib/python3.14/site-packages/grid2op/Backend/backend.py:395, in Backend.load_grid_public(self, path, filename)
    379 """
    380 INTERNAL
    381 
   (...)    392 
    393 """
    394 # first load the grid for the public part
--> 395 self.load_grid(path, filename)
    397 # and finish the initialization with a call to this function
    398 self._compute_pos_big_topo()

File ~/prjs_my/power-grid-model-benchmark/.venv/lib/python3.14/site-packages/grid2op/Backend/pandaPowerBackend.py:381, in PandaPowerBackend.load_grid(self, path, filename)
    378 self._iref_slack = None
    379 self._id_bus_added = None
--> 381 self._aux_run_pf_init()  # run an intiail powerflow, just in case
    383 new_pp_version = False
    384 if "slack_weight" not in self._grid.gen:

File ~/prjs_my/power-grid-model-benchmark/.venv/lib/python3.14/site-packages/grid2op/Backend/pandaPowerBackend.py:614, in PandaPowerBackend._aux_run_pf_init(self)
    612 warnings.filterwarnings("ignore")
    613 try:
--> 614     self._aux_runpf_pp(False)
    615     if not self._grid.converged:
    616         raise pp.powerflow.LoadflowNotConverged

File ~/prjs_my/power-grid-model-benchmark/.venv/lib/python3.14/site-packages/grid2op/Backend/pandaPowerBackend.py:1102, in PandaPowerBackend._aux_runpf_pp(self, is_dc)
   1100         self._nb_bus_before = None
   1101     else:
-> 1102         pp.runpp(
   1103             self._grid,
   1104             check_connectivity=False,
   1105             init=self._pf_init,
   1106             numba=self.with_numba,
   1107             lightsim2grid=self._lightsim2grid,
   1108             max_iteration=self._max_iter,
   1109             distributed_slack=self._dist_slack,
   1110         )
   1111 except IndexError as exc_:
   1112     raise pp.powerflow.LoadflowNotConverged(f"Surprising behaviour of pandapower when a bus is not connected to "
   1113                                             f"anything but present on the bus (with check_connectivity=False). "
   1114                                             f"Error was {exc_}"
   1115                                             ) from exc_

File ~/prjs_my/power-grid-model-benchmark/.venv/lib/python3.14/site-packages/pandapower/run.py:225, in runpp(net, algorithm, calculate_voltage_angles, init, max_iteration, tolerance_mva, trafo_model, trafo_loading, enforce_q_lims, check_connectivity, voltage_depend_loads, consider_line_temperature, run_control, distributed_slack, tdpf, tdpf_delay_s, **kwargs)
    223 else:
    224     passed_parameters = _passed_runpp_parameters(locals())
--> 225     _init_runpp_options(net, algorithm=algorithm,
    226                         calculate_voltage_angles=calculate_voltage_angles,
    227                         init=init, max_iteration=max_iteration, tolerance_mva=tolerance_mva,
    228                         trafo_model=trafo_model, trafo_loading=trafo_loading,
    229                         enforce_q_lims=enforce_q_lims, check_connectivity=check_connectivity,
    230                         voltage_depend_loads=voltage_depend_loads,
    231                         consider_line_temperature=consider_line_temperature,
    232                         tdpf=tdpf, tdpf_delay_s=tdpf_delay_s,
    233                         distributed_slack=distributed_slack,
    234                         passed_parameters=passed_parameters, **kwargs)
    235     _check_bus_index_and_print_warning_if_high(net)
    236     _check_gen_index_and_print_warning_if_high(net)

File ~/prjs_my/power-grid-model-benchmark/.venv/lib/python3.14/site-packages/pandapower/auxiliary.py:1722, in _init_runpp_options(net, algorithm, calculate_voltage_angles, init, max_iteration, tolerance_mva, trafo_model, trafo_loading, enforce_q_lims, check_connectivity, voltage_depend_loads, passed_parameters, consider_line_temperature, distributed_slack, tdpf, tdpf_update_r_theta, tdpf_delay_s, **kwargs)
   1719     numba = _check_if_numba_is_installed()
   1721 if voltage_depend_loads:
-> 1722     if not (np.any(net["load"]["const_z_p_percent"].values)
   1723             or np.any(net["load"]["const_i_p_percent"].values)
   1724             or np.any(net["load"]["const_z_q_percent"].values)
   1725             or np.any(net["load"]["const_i_q_percent"].values)):
   1726         voltage_depend_loads = False
   1728 lightsim2grid = _check_lightsim2grid_compatibility(net, lightsim2grid, voltage_depend_loads, algorithm,
   1729                                                    distributed_slack, tdpf)

File ~/prjs_my/power-grid-model-benchmark/.venv/lib/python3.14/site-packages/pandas/core/frame.py:4113, in DataFrame.__getitem__(self, key)
   4109 
   4110         if is_single_key:
   4111             if self.columns.nlevels > 1:
   4112                 return self._getitem_multilevel(key)
-> 4113             indexer = self.columns.get_loc(key)
   4114             if is_integer(indexer):
   4115                 indexer = [indexer]
   4116         else:

File ~/prjs_my/power-grid-model-benchmark/.venv/lib/python3.14/site-packages/pandas/core/indexes/base.py:3819, in Index.get_loc(self, key)
   3814     if isinstance(casted_key, slice) or (
   3815         isinstance(casted_key, abc.Iterable)
   3816         and any(isinstance(x, slice) for x in casted_key)
   3817     ):
   3818         raise InvalidIndexError(key)
-> 3819     raise KeyError(key) from err
   3820 except TypeError:
   3821     # If we have a listlike key, _check_indexing_error will raise
   3822     #  InvalidIndexError. Otherwise we fall through and re-raise
   3823     #  the TypeError.
   3824     self._check_indexing_error(key)

KeyError: 'const_z_p_percent'

## Error Message 3
---------------------------------------------------------------------------
EnvError                                  Traceback (most recent call last)
Cell In[13], line 32
     28     pp_backend.apply_action.reset()
     29     pp_backend.runpf.reset()
     30 
     31 
---> 32 benchmark_with_other_timer_and_return_result(
     33     run_g2o_pp,
     34     timer=time_g2o_pp,
     35     setup=setup_g2o_pp,

Cell In[9], line 53, in benchmark_with_other_timer_and_return_result(func, timer, method, calculation, is_batch, setup)
     49     calculation: Calculation,
     50     is_batch: bool,
     51     setup="pass",
     52 ):
---> 53     execution = benchmark_return_time_and_result(
     54         func=func, is_batch=is_batch, setup=setup
     55     )
     56 

Cell In[9], line 19, in benchmark_return_time_and_result(func, is_batch, setup)
     15     def _func():
     16         nonlocal result
     17         result = func()
     18 
---> 19     execution_time = timeit.repeat(
     20         _func,
     21         setup=setup,
     22         repeat=1 if is_batch else n_single_scenario_repeats,

File ~/.local/share/uv/python/cpython-3.14.3-linux-x86_64-gnu/lib/python3.14/timeit.py:240, in repeat(stmt, setup, timer, repeat, number, globals)
    237 def repeat(stmt="pass", setup="pass", timer=default_timer,
    238            repeat=default_repeat, number=default_number, globals=None):
    239     """Convenience function to create Timer object and call repeat method."""
--> 240     return Timer(stmt, setup, timer, globals).repeat(repeat, number)

File ~/.local/share/uv/python/cpython-3.14.3-linux-x86_64-gnu/lib/python3.14/timeit.py:205, in Timer.repeat(self, repeat, number)
    203 r = []
    204 for i in range(repeat):
--> 205     t = self.timeit(number)
    206     r.append(t)
    207 return r

File ~/.local/share/uv/python/cpython-3.14.3-linux-x86_64-gnu/lib/python3.14/timeit.py:177, in Timer.timeit(self, number)
    175 gc.disable()
    176 try:
--> 177     timing = self.inner(it, self.timer)
    178 finally:
    179     if gcold:

File <timeit-src>:6, in inner(_it, _timer, _setup, _stmt)
      2 'Could not get source, probably due dynamically evaluated source code.'

Cell In[9], line 17, in benchmark_return_time_and_result.<locals>._func()
     15     def _func():
     16         nonlocal result
---> 17         result = func()

Cell In[13], line 14, in run_g2o_pp()
     12 def run_g2o_pp():
     13     global g2o_pp_grid
---> 14     g2o_pp_grid = g2o.make(
     15         Path("g2o_grid_sym").absolute(),
     16         backend=pp_backend,
     17         data_feeding_kwargs={

File ~/prjs_my/power-grid-model-benchmark/.venv/lib/python3.14/site-packages/grid2op/MakeEnv/Make.py:429, in make(dataset, test, logger, experimental_read_from_local_dir, n_busbar, allow_detachment, _add_cls_nm_bk, _add_to_name, _compat_glop_version, _overload_name_multimix, **kwargs)
    424     if not "experimental_read_from_local_dir" in kwargs:
    425         kwargs[
    426             "experimental_read_from_local_dir"
    427         ] = experimental_read_from_local_dir
--> 429     return make_from_path_fn(
    430         dataset_path=dataset,
    431         _add_cls_nm_bk=_add_cls_nm_bk,
    432         _add_to_name=_add_to_name_tmp,
    433         _compat_glop_version=_compat_glop_version_tmp,
    434         _overload_name_multimix=_overload_name_multimix,
    435         n_busbar=n_busbar,
    436         allow_detachment=allow_detachment,
    437         **kwargs
    438     )
    440 # Not a path: get the dataset name and cache path
    441 dataset_name = _extract_ds_name(dataset)

File ~/prjs_my/power-grid-model-benchmark/.venv/lib/python3.14/site-packages/grid2op/MakeEnv/MakeFromPath.py:1065, in make_from_dataset_path(dataset_path, logger, experimental_read_from_local_dir, n_busbar, allow_detachment, _add_cls_nm_bk, _add_to_name, _compat_glop_version, _overload_name_multimix, **kwargs)
   1061         classes_path = this_local_dir.name
   1063 # Finally instantiate env from config & overrides
   1064 # including (if activated the new grid2op behaviour)
-> 1065 env = Environment(
   1066     **default_kwargs,
   1067      chronics_handler=data_feeding,
   1068     _allow_loaded_backend=allow_loaded_backend,
   1069     _read_from_local_dir=classes_path,
   1070     _local_dir_cls=this_local_dir,
   1071 )   
   1072 if do_not_erase_cls is not None:
   1073     env._do_not_erase_local_dir_cls = do_not_erase_cls

File ~/prjs_my/power-grid-model-benchmark/.venv/lib/python3.14/site-packages/grid2op/Environment/environment.py:223, in Environment.__init__(self, init_env_path, init_grid_path, chronics_handler, backend, parameters, name, n_busbar, allow_detachment, names_chronics_to_backend, actionClass, observationClass, rewardClass, legalActClass, voltagecontrolerClass, other_rewards, thermal_limit_a, with_forecast, epsilon_poly, tol_poly, opponent_space_type, opponent_action_class, opponent_class, opponent_init_budget, opponent_budget_per_ts, opponent_budget_class, opponent_attack_duration, opponent_attack_cooldown, kwargs_opponent, attention_budget_cls, kwargs_attention_budget, has_attention_budget, logger, kwargs_observation, observation_bk_class, observation_bk_kwargs, highres_sim_counter, _update_obs_after_reward, _init_obs, _raw_backend_class, _compat_glop_version, _read_from_local_dir, _is_test, _allow_loaded_backend, _local_dir_cls, _overload_name_multimix)
    220 self._observationClass_orig = observationClass
    222 # init the backend
--> 223 self._init_backend(
    224     chronics_handler,
    225     backend,
    226     names_chronics_to_backend,
    227     actionClass,
    228     observationClass,
    229     rewardClass,
    230     legalActClass,
    231 )

File ~/prjs_my/power-grid-model-benchmark/.venv/lib/python3.14/site-packages/grid2op/Environment/environment.py:332, in Environment._init_backend(self, chronics_handler, backend, names_chronics_to_backend, actionClass, observationClass, rewardClass, legalActClass)
    329 self.load_alert_data()
    331 # to force the initialization of the backend to the proper type
--> 332 self.backend.assert_grid_correct(
    333     _local_dir_cls=self._local_dir_cls)
    334 self.backend.is_loaded = True
    335 need_process_backend = True

File ~/prjs_my/power-grid-model-benchmark/.venv/lib/python3.14/site-packages/grid2op/Backend/backend.py:2405, in Backend.assert_grid_correct(self, _local_dir_cls)
   2402     orig_type._clear_grid_dependant_class_attributes() 
   2404 my_cls = type(self)
-> 2405 my_cls._add_internal_classes(_local_dir_cls)
   2406 self._remove_my_attr_cls()
   2408 # speed optim

File ~/prjs_my/power-grid-model-benchmark/.venv/lib/python3.14/site-packages/grid2op/Backend/backend.py:2421, in Backend._add_internal_classes(cls, _local_dir_cls)
   2419 cls._complete_action_class._add_shunt_data()
   2420 cls._complete_action_class._update_value_set()
-> 2421 cls.assert_grid_correct_cls()

File ~/prjs_my/power-grid-model-benchmark/.venv/lib/python3.14/site-packages/grid2op/Space/GridObjects.py:2205, in GridObjects.assert_grid_correct_cls(cls)
   2201     raise IncorrectNumberOfElements(err_msg)
   2204 # for names
-> 2205 cls._check_names()
   2207 if len(cls.name_load) != cls.n_load:
   2208     raise IncorrectNumberOfLoads("len(self.name_load) != self.n_load")

File ~/prjs_my/power-grid-model-benchmark/.venv/lib/python3.14/site-packages/grid2op/Space/GridObjects.py:1742, in GridObjects._check_names(cls)
   1740 if tmp.shape[0] != arr_.shape[0]:
   1741     nms = "\n\t - ".join(sorted(arr_))
-> 1742     raise EnvError(
   1743         f'Two {nm} have the same names. Please check the "grid.json" file and make sure the '
   1744         f"name of the {nm} are all different. Right now they are \n\t - {nms}."
   1745     )

EnvError: Grid2OpException EnvError "Two lines have the same names. Please check the "grid.json" file and make sure the name of the lines are all different. Right now they are 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - 
	 - ."
