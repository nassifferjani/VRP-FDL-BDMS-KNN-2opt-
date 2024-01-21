# VRP-FDL-BDMS-KNN-2opt-
Vehicle Routing Problem with Fixed Duration Length and Best Depot Multi-Selection using KNN heuristics and 2-opt heuristic for optimization
### Overview
As part of my internship at UQTR (University of Quebec at Trois-Rivières), I applied a methodology for solving the Vehicle Routing Problem (VRP)
### Code
### VRP-FDL-BDMS-KNN


      import pandas as pd


    def get_nearest_neighbor(current_city, unvisited, matrix):
    nearest_city = None
    min_distance = float('inf')
    for city in unvisited:
        if matrix[current_city][city] < min_distance:
            nearest_city = city
            min_distance = matrix[current_city][city]
    return nearest_city, min_distance

    def select_best_depot(depots, unvisited, matrix):
    avg_distance_for_depot = {}
    for depot in depots:
        total_distance = sum([matrix[depot][city] for city in unvisited])
        avg_distance_for_depot[depot] = total_distance / len(unvisited)
    return min(avg_distance_for_depot, key=avg_distance_for_depot.get)

    def VRP(matrix, depots, max_tour_distance):
    unvisited = set(matrix.keys()) - set(depots)
    total_distance = 0
    tours = []

    iteration_count = 0
    while unvisited:
        if iteration_count > 10:  # avoid infinite loops
            print("Breaking out of infinite loop after 10 iterations.")
            break

        depot = select_best_depot(depots, unvisited, matrix)
        current_city = depot
        current_tour = [depot]
        current_distance = 0

        while True:
            nearest_city, nearest_distance = get_nearest_neighbor(current_city, unvisited, matrix)
            if not nearest_city or current_distance + nearest_distance + matrix[nearest_city][depot] > max_tour_distance:
                break

            current_city = nearest_city
            current_tour.append(current_city)
            current_distance += nearest_distance
            unvisited.remove(current_city)

        current_tour.append(depot)
        current_distance += matrix[current_city][depot]
        print(f"Tour from {depot}: {current_tour} with distance {current_distance}")
        tours.append((depot, current_tour, current_distance))
        total_distance += current_distance

        iteration_count += 1

      print(f"Total distance travelled: {total_distance}")
       return tours 
         def calculate_cost(route, matrix):
    cost = 0
    for i in range(len(route) - 1):
        cost += matrix[route[i]][route[i+1]]
    return cost

    def two_opt(route, matrix):
    best_route = copy.deepcopy(route)
    improvement = True
    while improvement:
        improvement = False
        for i in range(1, len(route) - 2):
            for j in range(i+1, len(route) - 1):
                new_route = route[:i] + route[j:i-1:-1] + route[j+1:]
                if calculate_cost(new_route, matrix) < calculate_cost(best_route, matrix):
                    best_route = new_route
                    improvement = True
        route = best_route
    return best_route
 
    file_path = '/content/duration_df.xlsx'
    matrix_df = pd.read_excel(file_path, index_col=0)
     MATRIX = matrix_df.to_dict(orient='index')

    depots = ['0-0-0 ; 0-0-0','0-0-1 ; 0-0-1']

     MAX_TOUR_DISTANCE = 8
    Change this value to the desired maximum distance
    Storing results in the input_tours format
     input_tours = {}
     used_keys = set()

     for tour in tours:
      depot, path, cost = tour
    if cost != 0:
        key = generate_unique_key(depot, used_keys)
        used_keys.add(key)
        input_tours[key] = [path, cost]

      print(input_tours)
       tours = VRP(MATRIX, depots, MAX_TOUR_DISTANCE)



![GERZGRHZHERZ](https://github.com/nassifferjani/VRP-FDL-BDMS-KNN-2opt-/assets/91385813/834c4395-012e-489d-bb1e-53de5911899b)

  

### 2-OPT
    file_path = 'duration_df.xlsx'
     matrix= pd.read_excel(file_path, index_col=0)

     optimized_tours = {}
     for depot, tour_info in input_tours.items():
     optimized_route = two_opt(tour_info[0], matrix)
     optimized_cost = calculate_cost(optimized_route, matrix)
     optimized_tours[depot] = [optimized_route, optimized_cost]
     total_distance = sum([info[1] for _, info in optimized_tours.items()])
     print("La somme totale des distances est:", total_distance)

       print(optimized_tours)
       
![gzagrah](https://github.com/nassifferjani/VRP-FDL-BDMS-KNN-2opt-/assets/91385813/65478012-6e1d-4574-bd85-d21ebc602c35)

### Result
![zagzagzrgrz](https://github.com/nassifferjani/VRP-FDL-BDMS-KNN-2opt-/assets/91385813/0b032a99-49d6-42ee-9a51-93b5dfb9e26d)


