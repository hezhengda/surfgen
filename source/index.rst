.. surfgen documentation master file, created by
   sphinx-quickstart on Mon Feb 17 12:37:31 2020.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Welcome to surfgen's documentation!
===================================

The surfgen code is developed by Zheng-Da He. It can be used in generating arbitrary surface slab system.

Update:
#######

* Currently only works on pure metal surfaces

Code Example
############

Here is an example of the code:

.. code-block:: Python

    # This tutorial works on Cu surfaces.
    # If you want to use other surfaces. Please change the d_Cu

    lc_Cu = 3.579 # lattice parameter of Cu bulk
    d_Cu = lc_Cu / np.sqrt(2) # closest distance between two Cu atoms

    Cu_bulk = bulk('Cu', 'fcc', lc_Cu)

    # single-atom adsorbates --- OH
    mol_one_atom = molecule('OH')
    mol_index = 0

    # two-atom adsorbates --- CH2CH2CH2
    mol_CH2CH2CH2 = read('CH2CH2CH2.mol', format='mol')

    # two-atom adsorbates --- C2H4
    mol_C2H4 = read('C2H4.mol', format='mol')

    # multiple-atom adsorbates --- benzene
    mol_C6H6 = read('benzene.mol', format='mol')

    # check slab_generator function --- check OK!
    slab_list = slab_generator(bulk=Cu_bulk, miller_index=(1, 1, 1), slab_height=8.0, vacuum=18.0, supercell=(3, 3, 1))
    slab = slab_list[0]
    # for slab in slab_list:
    #     view(slab)

    # check surf_atom_finder --- check OK!
    surf_atoms = surf_atom_finder(slab)
    # c = FixAtoms(indices = [atom.index for atom in slab if atom.index in surf_atoms])
    # slab.set_constraint(c)
    # view(slab)
    # print(surf_atoms)

    # check connection_matrix --- check OK!
    connector, conn_coordinates, outer_atom_index = connection_matrix(slab, surf_atoms, d_Cu)
    # print('The connector matrix is:')
    # print(connector)
    # print('The conn_coordinates is:')
    # print(conn_coordinates)
    # print('The outer_atom_index is:')
    # print(outer_atom_index)

    # check find_all_ads_sites --- check OK!
    ads_sites = find_all_ads_sites(slab, connector, conn_coordinates, surf_atoms, d_Cu)
    # for site in ads_sites:
    #     site_dict = ads_sites[site]
    #     for index in site_dict:
    #         print('{} for {} is:'.format(index, site))
    #         print(site_dict[index])

    site = ads_sites['ontop']['location'][1]
    norm_vector = ads_sites['ontop']['norm_vector'][1]
    label = ads_sites['ontop']['label'][1]

    # check align_adsorbate_one_atom --- check OK!
    mol_one_atom = align_adsorbate_one_atom(slab, surf_atoms, site, norm_vector, label, mol_one_atom, mol_index, 'sp3')

    match_dict_CH2CH2CH2 = {
        '0': {
            'location': np.array([4.20622089,  0.97039162, 18.28182945]),
            'norm_vector': np.array([-0.04701866, -0.02718636,  0.99852398])
        },
        '2': {
            'location': np.array([ 5.46738537,  3.16126249, 18.40086522]),
            'norm_vector': np.array([-0.04701866, -0.02718636,  0.99852398])
        }
    }

    match_dict_C2H4 = {
        '0': {
            'location': np.array([4.20622089,  0.97039162, 18.28182945]),
            'norm_vector': np.array([-0.04701866, -0.02718636,  0.99852398])
        },
        '1': {
            'location': np.array([ 5.46738537,  3.16126249, 18.40086522]),
            'norm_vector': np.array([-0.04701866, -0.02718636,  0.99852398])
        }
    }

    match_dict_C6H6 = {
        '0': {
            'location': np.array([4.20061572,  5.35213335, 18.40086522]),
            'norm_vector': np.array([-0.04701866, -0.02718636,  0.99852398])
        },
        '1': {
            'location': np.array([2.93945124,  3.16126249, 18.28182945]),
            'norm_vector': np.array([-0.04701866, -0.02718636,  0.99852398])
        },
        '2': {
            'location': np.array([4.20622089,  0.97039162, 18.28182945]),
            'norm_vector': np.array([-0.04701866, -0.02718636,  0.99852398])
        },
        '3': {
            'location': np.array([6.73415503,  0.97039162, 18.40086522]),
            'norm_vector': np.array([-0.04701866, -0.02718636,  0.99852398])
        },
        '4': {
            'location': np.array([7.9953195 ,  3.16126249, 18.51990099]),
            'norm_vector': np.array([-0.04701866, -0.02718636,  0.99852398])
        },
        '5': {
            'location': np.array([6.72854985,  5.35213335, 18.51990099]),
            'norm_vector': np.array([-0.04701866, -0.02718636,  0.99852398])
        }
    }

    # check align_adsorbate_multiple_atoms
    mol_multi_atom = align_adsorbate_multiple_atoms(slab, surf_atoms, mol_C6H6, match_dict_C6H6)

    slab += mol_multi_atom
    view(slab)

.. toctree::
   :maxdepth: 2
   :caption: Contents:

   what_will_come
   modules
   history_surfgen

Indices and tables
==================

* :ref:`genindex`
* :ref:`modindex`
* :ref:`search`
