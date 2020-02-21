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

    # test

    from surfgen.surfgen import slab_generator, surf_atom_finder, connection_matrix, find_all_ads_sites
    from ase.visualize import view
    from ase.build import bulk, molecule
    from ase.constraints import FixAtoms

    Cu_bulk = bulk('Cu', 'fcc', 3.579)

    slab = slab_generator(Cu_bulk, (1, 0, 0), 4, 18.0, (2, 2, 1))

    bond_length = 2.53 # only for Cu, it can also be in a range

    H = molecule('H')
    O = molecule('O')
    N = molecule('N')
    Cl = molecule('Cl')

    slab_trial = slab[0]

    surf_atoms = surf_atom_finder(slab_trial)

    # set constraints, and fix atoms
    c = FixAtoms([atom.index for atom in slab_trial if atom.index in surf_atoms])
    slab_trial.set_constraint(c)

    connector, conn_coordinates, outer_atom_index = connection_matrix(slab_trial, surf_atoms, bond_length)

    # print(connector)
    # print(conn_coordinates)

    ads_sites = find_all_ads_sites(slab_trial, connector, conn_coordinates, surf_atoms, bond_length)

    for site in ads_sites:
        print('{} site has {} possible positions'.format(site, len(ads_sites[site])))
        for coord in ads_sites[site]:
            print('The location is: {}'.format(coord))

    for site in ads_sites['ontop']:
        O.set_positions([[site[0], site[1], site[2]]])
        slab_trial += O

    for site in ads_sites['bridge']:
        H.set_positions([[site[0], site[1], site[2]]])
        slab_trial += H

    for site in ads_sites['hollow']:
        N.set_positions([[site[0], site[1], site[2]]])
        slab_trial += N

    for site in ads_sites['four_fold']:
        Cl.set_positions([[site[0], site[1], site[2]]])
        slab_trial += Cl

    view(slab_trial)

.. toctree::
   :maxdepth: 2
   :caption: Contents:

   modules

Indices and tables
==================

* :ref:`genindex`
* :ref:`modindex`
* :ref:`search`
