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
    mol = molecule('OH')
    mol_index = 0

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
    mol = align_adsorbate_one_atom(slab, surf_atoms, site, norm_vector, label, mol, mol_index, 'sp3')

    slab += mol
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
